# **DAY-3 | Apache Tomcat Hands-On**

Apache Tomcat, often referred to simply as **Tomcat**, is an open-source web server and servlet container developed by the Apache Software Foundation. It is widely used for deploying and running Java servlets and JavaServer Pages (JSP) applications. Tomcat is lightweight, easy to use, and compatible with various operating systems, making it a popular choice for both development and production environments.

---
---

## **Real World Scenario**
### **Scenario:**
An e-commerce company wants to build and deploy their web application using **Apache Tomcat**.

### **How Tomcat Fits into the Workflow:**
1. **Development**:
   - The development team builds the Java-based e-commerce web application using servlets and JSPs.
   - Tomcat serves as the runtime environment for local testing and debugging.

2. **Testing**:
   - Testers deploy the application on a shared or dedicated Tomcat instance for thorough testing.

3. **Staging**:
   - The application is deployed on a staging environment that closely resembles production, using a dedicated Tomcat server.

4. **Production Deployment**:
   - After passing all tests, the application is deployed to the production environment where Tomcat handles incoming web requests and serves dynamic content.

5. **Scalability and Load Balancing**:
   - Multiple Tomcat instances are configured in a cluster.
   - A load balancer distributes incoming traffic across the cluster, ensuring high availability and fault tolerance.

6. **Monitoring and Management**:
   - Tomcat's built-in monitoring tools are used to track performance and health.

7. **Security**:
   - Tomcat supports SSL/TLS encryption for secure communication between clients and the server.

### **Outcome**:
Apache Tomcat provides a reliable, scalable, and secure platform for hosting the e-commerce website, streamlining deployment, management, and operation throughout the application's lifecycle.

---

## **Commands to Set Up Apache Tomcat**

### **Step 1: Download and Extract Tomcat**
```bash
cd /opt
sudo wget https://archive.apache.org/dist/tomcat/tomcat-9/v9.0.65/bin/apache-tomcat-9.0.65.tar.gz
sudo tar -xvf apache-tomcat-9.0.65.tar.gz



Step 2: Configure Users

cd /opt/apache-tomcat-9.0.65/conf
sudo vi tomcat-users.xml
Add the following line before the closing </tomcat-users> tag:

<user username="admin" password="admin1234" roles="admin-gui,manager-gui"/>
Step 3: Create Start and Stop Commands

sudo ln -s /opt/apache-tomcat-9.0.65/bin/startup.sh /usr/bin/startTomcat
sudo ln -s /opt/apache-tomcat-9.0.65/bin/shutdown.sh /usr/bin/stopTomcat
Step 4: Modify Context Files
For the Manager App:

sudo vi /opt/apache-tomcat-9.0.65/webapps/manager/META-INF/context.xml
Comment out the RemoteAddrValve:

<!--
<Valve className="org.apache.catalina.valves.RemoteAddrValve"
       allow="127\.\d+\.\d+\.\d+|::1|0:0:0:0:0:0:0:1" />
-->
For the Host Manager App:


sudo vi /opt/apache-tomcat-9.0.65/webapps/host-manager/META-INF/context.xml
Comment out the RemoteAddrValve:


<!--
<Valve className="org.apache.catalina.valves.RemoteAddrValve"
       allow="127\.\d+\.\d+\.\d+|::1|0:0:0:0:0:0:0:1" />
-->
Step 5: Start and Stop Tomcat

sudo stopTomcat
sudo startTomcat

Step 6: Deploy Your Application
To deploy your WAR file:

sudo cp target/*.war /opt/apache-tomcat-9.0.65/webapps/
```
![image](https://github.com/user-attachments/assets/f688b982-2882-4f59-86de-40b32c8f0fb8)
![image](https://github.com/user-attachments/assets/b6b57a1b-139f-4aac-ad57-36ebcd7c0e89)

