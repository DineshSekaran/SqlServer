WITH "Period" (PeriodM, PeriodName) AS (
    -- // I would store it as another table basically, but having it as part of the view would do
                SELECT  01, '1 mo'
    UNION ALL   SELECT  02, '2 mo' -- // data not stored
    UNION ALL   SELECT  03, '3 mo'
    UNION ALL   SELECT  06, '6 mo'
    UNION ALL   SELECT  12, '1 yr'
    UNION ALL   SELECT  24, '2 yr'
    UNION ALL   SELECT  36, '3 yr'
    UNION ALL   SELECT  48, '4 yr' -- // data not stored
    UNION ALL   SELECT  60, '5 yr'
    UNION ALL   SELECT  72, '6 yr' -- // data not stored
    UNION ALL   SELECT  84, '7 yr'
    UNION ALL   SELECT  96, '8 yr' -- // data not stored
    UNION ALL   SELECT 108, '9 yr' -- // data not stored
    UNION ALL   SELECT 120, '10 yr'
    -- ... // add more
    UNION ALL   SELECT 240, '20 yr'
    -- ... // add more
    UNION ALL   SELECT 360, '30 yr'
)
, "Yield" (ID, PeriodM, Date, Value) AS (
    -- // ** This is the TABLE your data is stored in **
    -- // 
    -- // value of ID column is not important, but it must be unique (you may have your PK)
    -- // ... it is used for a Tie-Breaker type of JOIN in the view
    -- //
    -- // This is just a test data:
                SELECT 101, 01 /* '1 mo'*/, '2009-05-01', 0.06
    UNION ALL   SELECT 102, 03 /* '3 mo'*/, '2009-05-01', 0.16
    UNION ALL   SELECT 103, 06 /* '6 mo'*/, '2009-05-01', 0.31
    UNION ALL   SELECT 104, 12 /* '1 yr'*/, '2009-05-01', 0.49
    UNION ALL   SELECT 105, 24 /* '2 yr'*/, '2009-05-01', 0.92
    UNION ALL   SELECT 346, 36 /* '3 yr'*/, '2009-05-01', 1.39
    UNION ALL   SELECT 237, 60 /* '5 yr'*/, '2009-05-01', 2.03
    UNION ALL   SELECT 238, 84 /* '7 yr'*/, '2009-05-01', 2.72
    UNION ALL   SELECT 239,120 /*'10 yr'*/, '2009-05-01', 3.21
    UNION ALL   SELECT 240,240 /*'20 yr'*/, '2009-05-01', 4.14
    UNION ALL   SELECT 250,360 /*'30 yr'*/, '2009-05-01', 4.09
)
, "ReportingDate" ("Date") AS (
    -- // this should be a part of the view (or a separate table)
    SELECT DISTINCT Date FROM "Yield"
)

-- // This is the Final VIEW that you want given the data structure as above
SELECT      d.Date, p.PeriodName, --//p.PeriodM,
            CAST(
                COALESCE(y_curr.Value,
                    (   (p.PeriodM - y_prev.PeriodM) * y_prev.Value
                    +   (y_next.PeriodM - p.PeriodM) * y_next.Value
                    ) / (y_next.PeriodM - y_prev.PeriodM)
                ) AS DECIMAL(9,4) -- // TODO: cast to your type if not FLOAT
            )  AS Value
FROM        "Period" p
CROSS JOIN  "ReportingDate" d
LEFT JOIN   "Yield" y_curr
        ON  y_curr.Date = d.Date
        AND y_curr.PeriodM = p.PeriodM
LEFT JOIN   "Yield" y_prev
        ON  y_prev.ID = (SELECT TOP 1 y.ID FROM Yield y WHERE y.Date = d.Date AND y.PeriodM <= p.PeriodM ORDER BY y.PeriodM DESC)
LEFT JOIN   "Yield" y_next
        ON  y_next.ID = (SELECT TOP 1 y.ID FROM Yield y WHERE y.Date = d.Date AND y.PeriodM >= p.PeriodM ORDER BY y.PeriodM ASC)
