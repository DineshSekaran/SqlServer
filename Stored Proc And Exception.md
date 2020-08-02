
# System Stored Procedure

It is like Metadata objects which is ued mainly for for rename,alter/vire stored code etc..
Like Sp_Rename,Sp_Help_inedx,sp_help...

This is created as deafult by Micrososft using installation


# User Stored Procedure

It is used for proejct Application specific stord created by user.

Group of SQl statement .

Stored Procedure will have return value by default that is Inetegr ie.. 0/1  0--> Success  1--> Error

Stored may have both Ouptut parameter and as well Return . But we can retuen only inetger and with only one value.

IN output parmeter we can return many columns.


# Return:

Only integer value . only 1 value . Used to get Success /Fialure.

# Output:

Any Data type. Used to get multiple column values.return more than 1 value


# To SEE STORED PROC CODE

sp_helptext sp_name   --> object Defintion

sp_help   --> to know object created date and also it variables

sp_depends  --> to check dependency objects


# Advanatages:
  Reusability
  Explain Plan
  Reduce Network
  Caching the execution pland and reuse the plan
  Avoid SQL injection attack
  
  
  
  # Exception
  
  Using Try /Catch  block as like programming language.
  
 User defined  RaiserError() Takes 3 input Error Message,Severity,State
 
 Error state should be between 1 and 255
 
 @@ERROR is used to rollback/Commit for exception handling
 
 @@ERROR ssyetm used to retain the value by assigning to varibale otherwise the system will lose its error(by reset to 0) of previous violaion error.
 Each Statemnet should be used at every stmnt of exception.
  
  
  # Syntax:
  
  
 BEGIN TRY
 
  ANy set of sql activity
  
  END TRY
  
  BEGIN CATCH
  
  ROLLABCK
  
  ERROR_lOG
  
  ERROR_NUMBER,ERROR_LINE...
  
  END CATCH
