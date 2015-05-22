shelving
=========

Shelving is a Java web application (or, rather, a part of a larger application) that was built using JSF and Hibernate.
The main dependency is on the Voyager database and on CAS (although authentication can be adjusted or removed via web.xml, of course).

shelving-parent documents the application and contains a binary file (if useful).

### Building

The application is structured as 2 Maven modules (shelving, shelving-web).
The following steps assume you have cloned the 2 modules from GitHub and you are in the shelving directory.

1. Build requirements: Java 6+, Apache Tomcat, Apache Maven.
2. Install Maven.
3. (Optional) Install Oracle and SqlServer drivers (jar files), as necessary, using Maven's install:install-file command.
3b. (Optional) Install locally the UI jar file in the resources folder using install:install-file. 
(Otherwise, the dependency is expected to be pulled from a local Nexus server.)
4. Build the project:

```
mvn clean install
```

5. Go to shelving-web directory:

```
mvn clean install -P prod
```


### Deployment

1.The resulting war file (in the target folder in shelving-web) can now be dropped into Tomcat. Rename the war to shelving.war.

System requirements: Oracle (aka Voyager), Sql Server (or MySql)

1. IP addresses would need to change per new server. Grep for chron.library.yale.edu, and adjust accordingly.
2. Change Tomcat conf/context.xml (for DB connections). Memory: -Xms512m -Xmx1024m -XX:MaxPermSize=512m
3. Log file location needs to be set in logback.xml files (grep for it).
4. Adjust hibernate settings if MySql is used.
5. Adjust CAS IP settings as necessary.


### Database schema and Tomcat configuration files

For Tomcat context.xml, the following settings should work:

```
        <Resource name="jdbc/oraclevger_reports" auth="Container" type="javax.sql.DataSource"
               maxActive="2" maxIdle="2" maxWait="10000"
               username="" password="" driverClassName="oracle.jdbc.driver.OracleDriver"
               url="jdbc:oracle:thin:@domain:1521:VGER"
               testOnBorrow="true"
               validationQuery="select count(*) from ITEM_BARCODE"/>
```

```
        <Resource name="jdbc/shelfscan_sqlserver" auth="Container" type="javax.sql.DataSource"
               maxActive="15" maxIdle="30" maxWait="10000"
               username="" password=""
               driverClassName="com.microsoft.sqlserver.jdbc.SQLServerDriver"
               url="jdbc:sqlserver://domain:1433;Datbasename=DatabaseName"
               testOnBorrow="true"
               validationQuery="select * from admin"  />
```

The database schema could be generated through auto.ddl.

### Project description

|Item       | Value     |
|-----------|-----------|
|Inception  | 2012      |


### Notes

1. The Maven pom file have a reference to local Maven Nexus repo. These can be removed.

## Authors

Osman Din