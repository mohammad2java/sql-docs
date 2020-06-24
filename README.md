#ORACLE:
----------------

#FUNCTIONS
	
	1) function listing
	-------------------
	SELECT * FROM ALL_OBJECTS WHERE OBJECT_TYPE IN ('FUNCTION');
	
	2) create user-define function
	----------------------------------
	CREATE OR REPLACE FUNCTION subdate(days NUMBER)
	   RETURN DATE
	   IS ret DATE;
	
	BEGIN
	 ret := SYSDATE - days;
	      --SELECT SYSDATE-days INTO ret FROM DUAL;
	      RETURN ret;
	
	END;
	
	Notes: use only sql-developer client and hit f5 or run-script (to compile) button next to RUN button.
	
	
	3) calling user-define function
	----------------------------------
	select function_name from dual;  --note you can use any tables instead of dual.


