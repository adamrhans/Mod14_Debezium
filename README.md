# Event-Driven Approach to CDC Using Debezium
---

#### Overview:
For this assignment, I was tasked with creating a Docker container which housed a network. This network was to allow for the sharing of data between a MySQL image and a Debezium image. Having these two images share a network allow Debezium to detect a change on the MySQL side and keep a transaction of all the sql queries hitting the database. This monitoring system is called a CDC and is used throughout data warehouses in order for applications to communicate when low-level changes are made to a database. The application uses Spring Boot as the application framework and implements Debezium to look for changes to the MySQL database.

#### Data Engineerng Tools Used:
- Docker Containers and Images
- Virtual Network inside a container
- MySQL Container based on a DOCKERFILE
- Debezium Container used for monitoring changes
- Spring Boot application framework

#### Project Setup:
This project doesn't run independently but served as a getting started with the concepts. In oder to execute this project, here is what I had to do for the setup. 
- Needed to establish a single Docker Network (from terminal ran the following command)
	```
	docker network create myCDCNetwork
	```
- There were two files needed for this project.
  - Dockerfile
  - employee.sql
- I then needed to create the MySQL Docker Image to read the employee.sql file to initialize the database
	```
	docker build -t mysqlmasterimg . 
	```
- Next, I created the Docker container and tied it to the network
	```
	docker run --rm --name mysqlserver -p 3306:3306 --network myCDCNetwork -d mysqlmasterimg
	```
- I then unpacked the contents of the Debezium Folder and created the Docker image
	```
	docker build -t debeziumimg .
	```
- Then, I set up the debezium container
	```
	docker run -it --rm --name debeziumserver --network myCDCNetwork debeziumimg bash
	```
- I had to run two commands within the debezium container to install nano
	```
	apt-get update
	apt-get install nano
	```
- To start Debezium monitoring I ran the following command from the /tmp folder
	```
	mvn spring-boot:run
	```
- Connect to the MySQL database
	```
	mysql -h localhost -u root -p MyNewPass employeedb
	```
- Run INSERT Command
	```	
	INSERT INTO employeedb.employee VALUES (2, "Mary", "Doe", "4351234354", "mary@doe.com");
	```
