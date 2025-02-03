# Attendance-API - POC


## Metadata

| **Author** | **Created on** | **Version** | **Last updated by** | **Last edited on** | **Reviewed BY** |
|------------|----------------|-------------|----------------------|---------------------|----------|

   

## Table of Contents

- [Introduction](#introduction)
- [Pre-requisites](#pre-requisites)
-  [Run Time Dependency](#Run-Time-Dependency)
- [Getting Started Step by Step ](#getting-started-Step-by-Step)
- [Usage](#usage)
- [Contact](#contact)
- [References](#references)



## Introduction

This document is a proof-of-concept (POC) for an attendance API. It demonstrates the core functionality and design of the API, providing a foundation for further development and integration.

## Pre-requisites
- https://github.com/MyGurukulam-p11/Documentation-P11/blob/main/OT-Microservices/Application/Attendance-API/Detailed-Documentation/README.md#pre-requisites

## System requirements
- https://github.com/MyGurukulam-p11/Documentation-P11/tree/main/OT-Microservices/Application/Attendance-API/Detailed-Documentation#system-requirements

## Run Time Dependency
- https://github.com/MyGurukulam-p11/Documentation-P11/blob/main/OT-Microservices/Application/Attendance-API/Detailed-Documentation/README.md#dependencies
## Getting Started Step by Step

To get started with the attendance API POC, follow these steps:

1. Update and upgrade your system using the following command.

  ```
  sudo apt update && sudo apt upgrade -y

  ```
   
2. Install Python 3.11 and PIP:
  
   Install Python and pip, which are required for managing dependencies.
   
   ```
   sudo apt install python3.11 python3.11-dev python3.11-venv -y
   ```
   <img width="948" alt="Screenshot 2024-11-13 at 11 40 40 PM" src="https://github.com/user-attachments/assets/3e07f295-a4b2-4b37-b3fc-fdd14bfea3c7">

   ```
   sudo apt install python3-pip -y
   ```
   <img width="923" alt="Screenshot 2024-11-13 at 11 41 32 PM" src="https://github.com/user-attachments/assets/185705fc-c866-4c66-b651-ded18016808a">

    
3. Install Poetry:
   
   Poetry will manage the project’s dependencies.

   ```
   curl -sSL https://install.python-poetry.org | python3 -
   ```
   ```
   export PATH="$HOME/.local/bin:$PATH"
   source ~/.bashrc
   ```
   <img width="823" alt="Screenshot 2024-11-13 at 11 42 37 PM" src="https://github.com/user-attachments/assets/612f2502-631d-4826-837f-c62f09dfa0bb">


4. Install PostgreSQL:
    
- https://github.com/MyGurukulam-p11/Documentation-P11/tree/main/OTMicroservices/Softaware/Postgresql/POC
   


5. Install Redis:
   
 - https://github.com/MyGurukulam-p11/Documentation-P11/tree/main/OT-Microservices/Softaware/Redis/POC

6. Clone the Repository:
    ```sh
    git clone https://github.com/OT-MICROSERVICES/attendance-api.git
    cd attendance-api
    ```
    
     


7. Install Liquibase:

    Liquibase will handle database migrations.

    ```
    sudo snap install liquibase
    ```

    ```
    liquibase --version
    ```
    <img width="864" alt="Screenshot 2024-11-13 at 11 46 09 PM" src="https://github.com/user-attachments/assets/edc6e6b0-d53e-4a24-9fa2-6a5786c44e69">


   

8. Set up Dependency Installation:

    Install dependencies specified by the project’s `pyproject.toml` using Poetry.

    ```
    poetry install
    ```
    <img width="770" alt="Screenshot 2024-11-13 at 11 49 14 PM" src="https://github.com/user-attachments/assets/f628e7a3-3c97-48d2-a39f-063ca2668722">




9. Configure Liquibase:
    
    Set up Liquibase with the database connection details.


    ```
    nano liquibase.properties
    ```
    Update with your server’s IP and add the class path to the JDBC driver:

    ```
   
    url=jdbc:postgresql://<your-server-ip>:5432/attendance_db
    username=postgres
    password=password
    classpath=/home/ubuntu/attendance-api/liquibase_lib/postgresql-42.5.4.jar

    ```

10. Create Database Change Log for Liquibase:

     Create a `db-changelog.xml` file to define database migrations.

    ```
    <?xml version="1.0" encoding="UTF-8"?>

    <databaseChangeLog
    xmlns="http://www.liquibase.org/xml/ns/dbchangelog"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.liquibase.org/xml/ns/dbchangelog
                        http://www.liquibase.org/xml/ns/dbchangelog/dbchangelog-3.8.xsd">

    <!-- Example changeSet -->
    <changeSet author="your_name" id="1" context="test">
        <createTable tableName="example_table">
            <column name="id" type="int">
                <constraints primaryKey="true" nullable="false"/>
            </column>
            <column name="name" type="varchar(255)">
                <constraints nullable="false"/>
            </column>
        </createTable>
    </changeSet>

    </databaseChangeLog>

    ```


11. Run Database Migrations:

    Apply the changes defined in `db-changelog.xml` using Liquibase.

    ```
    liquibase --changeLogFile=db-changelog.xml update

    ```

    Or run migrations using Make:
    ```
     make run-migrations
    ```
    <img width="1080" alt="image" src="https://github.com/user-attachments/assets/ed375d0d-372b-4123-9fad-58089e3ca1e7">



12. Run the Application:

    Start the application using gunicorn.

    ```
    gunicorn app:app --log-config log.conf -b 0.0.0.0:8080
    ```
    <img width="1218" alt="Screenshot 2024-11-14 at 12 29 47 AM" src="https://github.com/user-attachments/assets/3bad75e5-102b-422c-9d84-45dfd7dc7709">

## Usage
To check wether the api is working or not we can use below command , replace localhost with your ip address if you are running the api on cloud

```sh
     http://<your-ip>:8080/apidocs
```
<img width="1192" alt="image" src="https://github.com/user-attachments/assets/99fa9a4a-0912-48c1-83d3-2d9ac154fd4c">



We can do health checks also by doing:

```sh
    http://<your-ip>:8080/api/v1/attendance/health/detail
```
<img width="1312" alt="Screenshot 2024-11-14 at 1 18 12 AM" src="https://github.com/user-attachments/assets/509dfaf9-a582-4cdb-8224-19589b6f63a9">


## Contact

For questions or support, please contact:

|       Name       |            Email Address               |
|------------------|----------------------------------------|




## Additional Resources
## References
- [Liquibase](https://www.liquibase.com/download)
- [Postgresql](https://www.postgresql.org/download/linux/ubuntu/)
- [Redis](https://redis.io/docs/latest/operate/oss_and_stack/install/install-redis/install-redis-on-linux/)

