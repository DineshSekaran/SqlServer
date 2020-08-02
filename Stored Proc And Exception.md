
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
  
  
