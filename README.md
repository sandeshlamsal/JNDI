# JNDI  is like setting the values of some variables that is not dependent on any application on war files, but is set in
application server or web server like tomcat,jboss etc.

uses the context to set the variables.
http://stackoverflow.com/questions/34062435/when-to-put-configuration-in-file-properties-or-jndi

The Java Naming and Directory Interface (JNDI) is an api to handle resources like db name, directories  on application server.
1.eg: DB data sources (test, production, developement evnironment)


Option 1 (DB setup in WAR)

Create a database.properties :

db.url=jdbc:mysql://localhost:3306/localdb
db.user=myusername
db.pass=mysecretpassword

#db.url=jdbc:mysql://10.1.2.3:3306/testdb
#db.user=myusername
#db.pass=mysecretpassword

#db.url=jdbc:mysql://10.2.3.4:3306/livedb
#db.user=myusername
#db.pass=mysecretpassword
Before you deploy it somewhere, you need to check if your settings are pointing to the right DB!

Also, if you check this file in to some version control system, then you might not want to publish your DB username/password to your local machine.

Option 2 (DB setup in App Server)

Imagine you have configured the three servers with their individual DB settings, and each of them registers the DB with the JNDI path java:database/mydb.

Then you can retrieve the DataSource like so:

Context context = new InitialContext();
DataSource dataSource = (DataSource) context.lookup("java:database/mydb");
This is working on every app server instance and you can deploy your WAR without the need to modify anything.
