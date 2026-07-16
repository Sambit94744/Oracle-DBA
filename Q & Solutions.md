# PARAMETER & PASSWORDFILE:

# 1.	Create pfile from spfile to location '/home/oracle/init<sid.ora>'

<img width="1122" height="260" alt="image" src="https://github.com/user-attachments/assets/e751058f-6f93-417a-8e55-e2c43606ab46" />

# 2. Create password file with sys password as oracle1

<img width="1139" height="170" alt="image" src="https://github.com/user-attachments/assets/e141dbd0-aadf-49a5-8344-d3dedc98a2d3" />

# 3. Create parameterfile in different location /u01 & start the DB to up and running condition.

SQL> CREATE PFILE='/u01/initORCL.ora' FROM SPFILE;

SQL> SHUTDOWN IMMEDIATE;

SQL> STARTUP PFILE='/u01/initORCL.ora';

SQL> SELECT status FROM v$instance;

<img width="510" height="266" alt="image" src="https://github.com/user-attachments/assets/c4a30ba4-0c7a-4ccd-b2d5-fea3b24c0166" />
<img width="374" height="410" alt="image" src="https://github.com/user-attachments/assets/0b275cd4-85c8-486c-9ede-f11a01e6ef77" />

# 4. How to check all parameters associated with database?

SHOW PARAMETER;

# 5. How to change specific parameters with Scope=both/spfile/memory?

   ALTER SYSTEM SET parameter_name = value SCOPE = {MEMORY | SPFILE | BOTH};
   
•	SCOPE=BOTH  -  Changes the parameter immediately in memory.

•	SCOPE=MEMORY  -  Changes the parameter only in memory.

•	SCOPE=SPFILE  -  Saves the change only in the SPFILE & the change takes effect after the next database restart.

# 6. How to check which parameter files required DB bounce for the changes to get reflected?
Query V$PARAMETER and we have to check the ISSYS_MODIFIABLE column. If it is FALSE, the parameter is static and requires a database restart for the change to take effect. Dynamic parameters show IMMEDIATE or DEFERRED and do not require a database bounce.

SELECT name,value,issys_modifiable FROM v$parameter ORDER BY name;
<img width="849" height="752" alt="image" src="https://github.com/user-attachments/assets/f0782538-708d-4234-aa50-c87b408e484c" />




    
# 7. How to check which parameter files which can be change dynamically?
Query the V$PARAMETER view and check the ISSYS_MODIFIABLE column. Parameters with values IMMEDIATE or DEFERRED are dynamic and can be changed without restarting the database. Parameters with FALSE are static and require a database bounce.

SELECT name,value,issys_modifiable FROM v$parameter WHERE issys_modifiable IN ('IMMEDIATE', 'DEFERRED') ORDER BY name;
<img width="1097" height="687" alt="image" src="https://github.com/user-attachments/assets/e4060575-a3c6-45ea-aff9-3e7fb42c5007" />


# 8. How to check which parameter files cannot be changed?

Query V$PARAMETER and check the ISSYS_MODIFIABLE column. If it is FALSE, the parameter is static and cannot be changed dynamically. Changes must be written to the SPFILE (or PFILE) and the database must be restarted for them to take effect.

SELECT name,value,issys_modifiable FROM v$parameter WHERE issys_modifiable = 'FALSE' ORDER BY name;

<img width="1039" height="617" alt="image" src="https://github.com/user-attachments/assets/2f134533-9385-4e8d-b39d-1ddf82518094" />


# PROCESS & SESSION:

# 1.	How to check total/active/inactive session?

SELECT status, COUNT(*) FROM v$session GROUP BY status;
SELECT status, COUNT(*) AS session_count FROM v$session GROUP BY status;
SELECT sid,serial#,username,status,machine, program FROM v$session WHERE status = 'INACTIVE';
SELECT sid,serial#,username,status,machine, program FROM v$session WHERE status = 'ACTIVE';

<img width="1152" height="595" alt="image" src="https://github.com/user-attachments/assets/5e5faa6c-af45-428f-b6b3-9f380accbcd4" />


