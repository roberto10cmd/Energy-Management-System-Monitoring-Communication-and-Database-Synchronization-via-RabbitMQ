# Energy Management System - Monitoring, Communication, and Database Synchronization via RabbitMQ

## Description
This project implements a **Monitoring and Communication Microservice** for an **Energy Management System**. The microservice processes energy consumption data from smart meters, stores it in a PostgreSQL database, and sends alerts when the consumption exceeds the set limits. The data from smart meters is simulated through a separate application that reads from the `sensor.csv` file and sends measurements in JSON format to a message broker. Synchronization between the Device Management and Monitoring microservices is achieved via an event-based system, where device changes are communicated through a message queue (RabbitMQ).

## Features

### 1. Authentication and Authorization
- **Authentication:** Users authenticate using a standard login system (e.g., email and password).
- **Role-based Authorization:** After authentication, the user's role (admin or client) determines which resources they can access. 
  - The **admin** role allows full CRUD operations on user accounts and devices.
  - The **client** role restricts access to only viewing associated devices.

#### Flow:
- The user logs in with the appropriate credentials.
- Based on the role (admin or client), the user is redirected to the relevant dashboard.
- Role-based access to resources is managed through middleware.

### 2. Administrator Role
- **CRUD Operations on User and Device Accounts:**
  - **Create:** The admin can add new users and devices.
  - **Read:** The admin can view all information about users and devices.
  - **Update:** The admin can update user and device details, including device allocation to users.
  - **Delete:** The admin can delete users and devices. Deleting a device will trigger the deletion of associated measurements.
  
- **Device Allocation to Users:** The admin can assign measurement devices to existing users.

### 3. Client Role
- **View Associated Devices:** Each client has a dedicated dashboard where they can view only the devices associated with their account. 
  - The dashboard displays device status, total energy consumption per hour, and alerts for consumption limits being exceeded.
  - Clients receive real-time WebSocket notifications when a device exceeds its consumption limit.

## Technical Requirements

- **Backend:**
  - Three microservices implemented using **Java Spring Boot**:
    1. **User Management Service** – manages users and authentication.
    2. **Device Management Service** – manages user devices.
    3. **Monitoring Management Service** – manages smart meters' consumption.
  
- **Frontend:** A **React** application for the user interface.
- **Database:** **PostgreSQL** hosted on **Microsoft Azure**.

## Environment Setup

### Prerequisites
- **Node.js** (v14+)
- **Java JDK** (v17)
- **Docker** (for containerization)
- **PostgreSQL** (compatible version for Microsoft Azure)

### Project Setup

#### Backend Configuration:
Each service contains a `spring boot` **`application.properties`** file with database connection details and service ports.

Example:
```properties
server.port=8081
spring.datasource.url=jdbc:postgresql://postgres-container- user:5433/myDB
spring.datasource.username=USER
spring.datasource.password=PASSWORD

