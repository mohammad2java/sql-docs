##MYSQL
----------------

		0.0) remote access of mysql/mariadb
		----------------------------
		 bydefault when you setup mysql it will accessible from same host(local) ,
		 user cant not access from remote because of bind-address default value is :127.0.0.1
		 if you change the bind-address = 0.0.0.0 means it will be accessible from all ips
		 you can put single ip or multiple range of ips its upto you as admin.
		 
		 0.1) how to check current bind-address
		 show variables like '%bind%';
		 
		 0.2) how to change it 
		 -----------------
		 you need to find the location of mysql conf file which is my.cnf
		 when you check status of running mysql using:  systemctl status mysql
		  you will see location of file like 
		   /system.slice/mariadb.service
           		└─24448 /usr/sbin/mysqld --defaults-file=/etc/my.cnf --user=mysql
		 here config file location is : /etc/my.cnf

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
	
	5) reset password expiry to unlimited 
	
	select * from dba_profiles where RESOURCE_NAME LIKE 'PASSWORD_LIFE_TIME';
	ALTER PROFILE <profile_name> LIMIT PASSWORD_LIFE_TIME UNLIMITED; 

	6) Reset Oracle User Password-( First login as System or SYSDBA user and run below ).
		ALTER USER <user> IDENTIFIED BY <password> ACCOUNT UNLOCK;
		
		
	7) changing httpport(default 8080) to 8010 
	solutions: login as system user and Exec DBMS_XDB.SETHTTPPORT(3010);
	program files>Run Sql Command Line> connect command>put user system and put correct password(used when installed)> run above proceedure.
	
	
	
