##MYSQL
----------------


		1) type of licence of mysql/mariadb
		----------------------------
		SELECT @@license
		
		
		2) running port of mysql/,mariadb
		--------------------------------
		SHOW GLOBAL VARIABLES LIKE 'PORT';
		
		
		3) How to see list of users:
		----------------------------
		SELECT User,Host FROM mysql.user
		
		
		4) how to check grants of users
		----------------------------
		4.1) for current login user;
		----------------------
		show grants
		
		4.2) for specific users
		----------------------------
		show grants for 'username'@'hostaddress'
		
		for all host it will %
		example
		show grants for 'username'@'%'
		show grants for 'username'@'localhost'
		
		5) how to apply grants for existing users
		---------------------------------------
		
		GRANT SELECT PRIVILEGES ON dbname.* TO 'username'@'%';
		
		GRANT ALL ON db1.* TO 'jeffrey'@'localhost';
		
		
		6) create users without grants:
		-----------------------------
		CREATE USER 'jeffrey'@'localhost' IDENTIFIED BY 'password';
		
		
		7) create/Replace new users with grants:
		
		GRANT ALL PRIVILEGES ON *.* TO 'username'@'localhost' IDENTIFIED BY 'password';
		
		GRANT SELECT ON *.* TO 'username'@'localhost' IDENTIFIED BY 'password';   for read access only.
		
		Notes:
		localhost: Means this user can be access from localhost. to access from everywhere must be use % instead of localhost.
		
		GRANT SELECT ON *.* TO 'dbname'@'%' IDENTIFIED BY 'password'; 
		
		
		
		8) how to drop users:
		========================
		
		DROP USER 'username'@'localhost';
		
		example:
		DROP USER 'username'@'%';
		
		9) login mysql user from putty/cmd:
		---------------------------------------
		mysql --user=root --password=admin 
		mysql -uUSERNAME -pPASSWORD
		mysql -uUSERNAME -p   it will ask password
		mysql -uUSERNAME     for user who dont have password
		
			
		
		10) MySQL explain keyword And indexes
		========================================
		10.1) To show the tables indexes
		----------+++++++++++++++/-----
		SHOW INDEXES FROM table_name;
		
		10.2) To create indexes
		----------+++++++++++------
		CREATE INDEX id_index ON table_name(column_name)
		
		
		10.3) To drop indexes
		-----+++++------------
		DROP INDEX `index_id` ON `table_name`;
		
		
		
		
		11) To Show function and procedure list schema wise and views
		===============================================
		SHOW FUNCTION STATUS;
		SHOW PROCEDURE STATUS;
		SHOW FULL TABLES WHERE table_type = 'VIEW';
		
		for both using root access
		=================================
		select name ,type from mysql.proc
		
		12) display code of procedure/function
		================================----------------------------
		Example
		
		SHOW CREATE PROCEDURE dbaame.procedurename;
		SHOW CREATE FUNCTION  dbname.functioname
		
		
		
		13) status monitering
		=================================
		SHOW Global STATUS like '%Threads%'
		
		show table status
		
		select @@version
		
		
		
		14) for trasaction start with begin and end with commit/rollback:	
		
		begin;
	
		//script	
		commit/rollback;
		
		
		
		15) Binary Logging in mysql( for all operation need root access)
		============================================================
		use for replication and recovery of database.
		
		15.1) How to enable binary log
		----------------------------
		add follwing entry in my.ini/my.cnf file.
		# Binary Logging
		log-bin="amirlog-bin"
		
		where amirlog-bin base file name of binary logs.
		
		default storage location is datadir. you can find datadir path in my.ini file.
		example:
		#Path to the database root
		datadir="C:/ProgramData/MySQL/MySQL Server 5.5/Data/"
		
		
		15.2) How to show list of binary log
		show binary logs;
		
		15.3) How to show list of binary logs (slave which is currently associated to mysql engine)
		 show slave status;
		 
		 Noted --dont purge slave status list of logs
		 
		 
		 16) How to purge binary logs file.
		 ----------------------------
		 
		 1) for single file
		 PURGE BINARY LOGS TO 'filename';
		 
		 2) for multiple files
		 PURGE BINARY LOGS BEFORE '2018-01-01 22:46:26';
		 
		
		17) MYSQL tables casesensitive or incasesesitive
		----------------------------------------------
		show global variables where VARIABLE_NAME like '%lower_case_table_names%';
		
		MariaDB [(none)]> show global variables where VARIABLE_NAME like '%lower_case_table_names%';
		+------------------------+-------+
		| Variable_name          | Value |
		+------------------------+-------+
		| lower_case_table_names | 0     |
		+------------------------+-------+
		
		0 means sensitive
		1 means in-sensitive
		  
		 
		

##ORACLE:
----------------

#FUNCTIONS
	
	1) function listing
	-------------------
	SELECT * FROM ALL_OBJECTS WHERE OBJECT_TYPE IN ('FUNCTION');
	
	2) create user-define function
	----------------------------------
	SYNTAX-
	------
		CREATE [OR REPLACE] FUNCTION function_name
		   [ (parameter [,parameter]) ]
		
		   RETURN return_datatype
		
			IS | AS
		
		   [declaration_section]
		
			BEGIN
		   executable_section
		
			[EXCEPTION
		   	exception_section]
		
			END [function_name];
	
	
	Example
	----------
	CREATE OR REPLACE FUNCTION subdate(days NUMBER)
	   RETURN DATE
	IS 
	   ret DATE;
	
	BEGIN
	 ret := SYSDATE - days;
	      --SELECT SYSDATE-days INTO ret FROM DUAL;
	      RETURN ret;
	
	END;
	
	Notes: use only sql-developer client and hit f5 or run-script (to compile) button next to RUN button.
	  here := is assignment operator
	
	3) calling user-define function
	----------------------------------
	select function_name from dual;  --note you can use any tables instead of dual.



	4) drop the function
	
	DROP FUNCTION function_name;  
	
	
