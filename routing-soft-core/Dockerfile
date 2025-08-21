# Use Tomcat with JDK 17
FROM tomcat:10.1-jdk17

# Remove default ROOT app
RUN rm -rf /usr/local/tomcat/webapps/*

# Copy WAR file
COPY target/routing.war /usr/local/tomcat/webapps/ROOT.war

# Expose Tomcat
EXPOSE 8080

CMD ["catalina.sh", "run"]

