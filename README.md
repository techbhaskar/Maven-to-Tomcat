# Maven-to-Tomcat
How to deploy war file from maven to tomcat

Requirements:

    Maven
    
    Tomcat server 7/8/9, Here I am using Tomcat server 9

Step 1: For Tomcat authentication used the existing users

%TOMCAT7_PATH%/conf/tomcat-users.xml

      <role rolename="manager-gui"/>
        <role rolename="manager-script"/>
        <user username="admin" password="password" roles="manager-gui,manager-script" />
      

Step 2: Link the Tomcat user to Maven for authentication from Maven

%MAVEN_PATH%/conf/settings.xml

      <server>
        <id>TomcatServer</id>
        <username>admin</username>
        <password>password</password>
      </server>
      
 
Step 3: Add the tomcat plug-in code the application pom.xml file

        <plugin>
            <groupId>org.apache.tomcat.maven</groupId>
            <artifactId>tomcat7-maven-plugin</artifactId>
            <version>2.2</version>
            <configuration>
              <url>http://localhost:8080/manager/text</url>
              <server>TomcatServer</server>
              <path>/springmvc</path>
              <username>admin</username>
        	  <password>password</password>
            </configuration>
         </plugin>

Step 4: Start the Tomcat server, If you are doing it from commmand prompt use the below command
goto -> %TOMCAT_BASE%/bin and then run
        startup.bat
      
Step 5: To deploy the war file directly into tomcat server use the below command

mvn tomcat7:deploy 

mvn tomcat7:undeploy

mvn tomcat7:redeploy

      [INFO] --- tomcat7-maven-plugin:2.2:deploy (default-cli) @ spring-mvc ---
      [INFO] Deploying war to http://localhost:8080/springmvc
      Uploading: http://localhost:8080/manager/text/deploy?path=%2Fspringmvc
      Uploaded: http://localhost:8080/manager/text/deploy?path=%2Fspringmvc (4722 KB a
      t 959.8 KB/sec)

      [INFO] tomcatManager status code:200, ReasonPhrase:
      [INFO] OK - Deployed application at context path [/springmvc]
      [INFO] ------------------------------------------------------------------------
      [INFO] BUILD SUCCESS
      [INFO] ------------------------------------------------------------------------
      [INFO] Total time: 12.934 s
      [INFO] Finished at: 2019-05-06T00:46:57+05:30
      [INFO] ------------------------------------------------------------------------


# Note : 
To deploy into tomcat server 6, we have to do a small correction the plugin code in pom.xml file

        <plugin>
            <groupId>org.apache.tomcat.maven</groupId>
            <artifactId>tomcat6-maven-plugin</artifactId>
            <version>2.2</version>
            <configuration>
              <url>http://localhost:8080/manager</url>
              <server>TomcatServer</server>
              <path>/springmvc</path>
            </configuration>
         </plugin>

To deploy

mvn tomcat6:deploy 

mvn tomcat6:undeploy 

mvn tomcat6:redeploy 

