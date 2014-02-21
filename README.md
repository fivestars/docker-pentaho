docker-pentaho
==============

### Overview
Pentaho is an open source business intelligence (BI) toolkit. They distribute an all-in-one, precompiled BI server for public use. Under the hood, it also comes with binaries/JARs for:
- Tomcat, which runs the server
- A Java JDBC postgres driver (that supports PG 9.1)
- A number of components of the Pentaho open source world, including the Mondrian engine (where data is loaded into a memory-based store for all the BI analysis), Kettle (Pentaho's ETL)

Docker is like a VM without the overhead. It uses linux containers to create fully isolated OS environments. In multi-tenant Java server environments, this isolation prevents all sorts of possible version and path conflicts.

### Quick start
1. Install docker: http://docs.docker.io/en/latest/installation/ubuntulinux/
2. Git checkout this repo
3. Copy your public key into the /app directory. Make sure it's an id_rsa.pub file, with that exact name
4. Go to the /app directory and run sudo docker build . yourname/pentaho
5. run sudo docker run yourname/pentaho &
6. sudo docker ps, then read the ID of the newly opened container
7. sudo docker inspect, and read the IP address and the <port> corresponding to 8080 at the end
8. Navigate your web browser to localhost:<port> and viola!

### TODO's
- https://github.com/jwarlander/docker-pentaho-ba-5 [apply some concepts from here]
- set up pentaho admin portal
