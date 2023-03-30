## Goal of the project

- Continous delivery for containers(Docker)to kubernetes clusters

## Prerequisites
- JDK 1.8 or later
- Maven 3 or later
- MySQL 5.6 or later

## Tools used

- kubernetes (Conatiner Orchestration )
- Docker (Container runtime)
- Jenkins (CI/CD)
- Docker Hub (Container Registry)
- Helm (Packaging and deploying on kubernetes)
- Git (Version Control)
- Maven (Build tool)
- SonarQube (Code Analysis)
- AWS (Cloud Environment) 
- EC2 Instance (Servers)
- ELB (Load balancer)

## Jenkins plugins

- Docker pipeline
- pipeline Utility steps
- slack
- Github integration plugins
- Maven integration plugins
- Nexus Artifact Uploader
- sonarqube sanner for jenkins
- Build timestamp


## Technologies 
- Spring MVC
- Spring Security
- Spring Data JPA
- Maven
- JSP
- MySQL
## Database
Here,we used Mysql DB 
MSQL DB Installation Steps for Linux ubuntu 14.04:
- $ sudo apt-get update
- $ sudo apt-get install mysql-server

Then look for the file :
- /src/main/resources/accountsdb
- accountsdb.sql file is a mysql dump file.we have to import this dump to mysql db server
- > mysql -u <user_name> -p accounts < accountsdb.sql


