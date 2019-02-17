# spring-examples
I tried to keep some good example related to spring, which might be helpful for other developer.Please make your comment so that, it's inspired me to post new topics.

Topic #1 :: DELETE HTTP Method with Request and Response body.

This example used spring-boot to achieve http delete method with payload.
To run this application on your weblogic 12.1.3 and Tomcat 9.0.14, we have to do some server configuration to achieve this.

# Weblogic level configuration
for weblogic configuration , we have to add below line in your application weblogic xml file, which will tell weblogic to use sun package HTTP connection instead of his own HTTP connection, since weblogic is not allowing the HTTP.DELETE with body, it is throwing Protocol exception.
<container-descriptor>
        <prefer-web-inf-classes>false</prefer-web-inf-classes>
        <prefer-application-packages>
           <package-name>sun.net.www.protocol.http.*</package-name>
        </prefer-application-packages>
</container-descriptor> 

# Tomcat level configuration
for tomcat configuration, we have to add initi-param "readonly" with false value, so that it allows to handle HTTP.DELETE method.

apache-tomcat-9.0.14\conf\web.xml, under this file add init-param for default server
<servlet>
    <servlet-name>default</servlet-name>
    <servlet-class>org.apache.catalina.servlets.DefaultServlet</servlet-class>
    <init-param>
        <param-name>readonly</param-name>
        <param-value>false</param-value>
    </init-param>
    ....
</servlet>

# Apache Server level configuration
This configuration required, if you are making ajax call with HTTP.DELETE call.

File location : Apache24\conf\extra\httpd-vhosts.conf

Add below header under <VirtualHost> element
        Header always set Access-Control-Allow-Methods "GET, POST, PUT, PATCH, OPTIONS,DELETE"
        Header always set Access-Control-Request-Method "GET,POST,PATCH,PUT,OPTIONS,DELETE"

