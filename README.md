# 🎯 Lab 1: Installation of MS SQL server. Data Bases files management. Managing SQL Server Security. Auditing Data Access and Encrypting Data

## 💡 Introduction
The DBMS Semester 2 curriculum involves learning how to administer, maintain and manage data warehouse infrastructure. As part of the curriculum update, it was decided to provide students with the opportunity to perform laboratory work using containerisation tools instead of virtualisation tools. This series of GitHub materials will provide a methodological guide for performing basic tasks from the mandatory labs using Docker containers with MS SQL Server on board.

## 🔗 Resources


## 📚 Objective
Deploying two SQL Server instances in docker containers, setting up authentication, security auditing, and setting up backups.

## 📝 Tasks

### 📌 Task 1: Installing independent instances of MS SQL Server
- There are SQL instances on the VMs. One is named, second is default. Read Windows feature installation menu – there is an option to use ‘sources’ folder from image.

### 📌 Task 2: Local connection to SQL engine
- “Sa” SQL login and windows user can connect to SQL engine via SSMS locally on each VM. Execution of “select @@SERVERNAME” returns right instance names.

### 📌 Task 3: Mounting backups on a disc
- At least “backups” folders are mapped to the location not in the system drive (Server properties – Database settings).

### 📌 Task 4: Cross-connection between machines
-	Student can connect from one machine to another using SQL login, and connect to both servers from Student host. RDP from Student host to VMs also available not via Hyper V tools. Windows firewall is active on both VMs. 

### ⚡ Extra task 1: Running Simple Spark Programs
- PowerShell module SQLServer or SQLPS is installed on local student computer.

### ⚡ Extra task 2: Running Simple Spark Programs
- Windows Firewall on VMs is active.

### ⚡ Extra task 3: Running Simple Spark Programs
- SSMS is installed on local student computer.

## 📙 Manual

## 🧠 Self-Check Questionnaire

### 🔍 Question 1:
Text.

### 🔍 Question 2:
Text.

### 🔍 Question 3:
Text.

### 🔍 Question 4:
Text.

### 🔍 Question 5:
Text.
