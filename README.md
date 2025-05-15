# 🎯 Lab 1: Installation of MS SQL server. Data Bases files management. Managing SQL Server Security. Auditing Data Access and Encrypting Data

## 💡 Introduction
The DBMS Semester 2 curriculum involves learning how to administer, maintain and manage data warehouse infrastructure. As part of the curriculum update, it was decided to provide students with the opportunity to perform laboratory work using containerisation tools instead of virtualisation tools. This series of GitHub materials will provide a methodological guide for performing basic tasks from the mandatory labs using Docker containers with MS SQL Server on board.

**The report to the laboratory work should be prepared in Microsoft Word according to ***СТП 2024*** and attached in .pdf format to the repository files.**

> Happy Hunger Games and may the odds be ever in your favor!

## 🔗 Resources

- [СТП 2024](https://www.bsuir.by/m/12_100229_1_185586.pdf)

- [Инструкция по установке Docker Desktop и развёртыванию вашего первого контейнера с MS SQL Server](https://github.com/poostotel-bsuir/laba-1/blob/main/%D0%98%D0%BD%D1%81%D1%82%D1%80%D1%83%D0%BA%D1%86%D0%B8%D1%8F_%D0%BF%D0%BE_MS_SQL_%D0%BD%D0%B0_%D0%B1%D0%B0%D0%B7%D0%B5_Docker_Desktop.docx)

- [Docker Desktop](https://www.docker.com/products/docker-desktop/)

- [Creating SQL Server Native Client Tables](https://learn.microsoft.com/en-us/sql/relational-databases/native-client-ole-db-tables-indexes/creating-sql-server-tables?view=sql-server-ver15)

- [Install and configure SQL Server on Windows from the command prompt](https://learn.microsoft.com/en-us/sql/database-engine/install-windows/install-sql-server-from-the-command-prompt?view=sql-server-ver15)

- [Install SQL Server using a configuration file](https://learn.microsoft.com/en-us/sql/database-engine/install-windows/install-sql-server-using-a-configuration-file?view=sql-server-ver15)

- [Install SQL Server with SysPrep](https://learn.microsoft.com/en-us/sql/database-engine/install-windows/install-sql-server-using-sysprep?view=sql-server-ver15)

- [Move a TDE protected database to another SQL Server](https://learn.microsoft.com/en-us/sql/relational-databases/security/encryption/move-a-tde-protected-database-to-another-sql-server?view=sql-server-ver15)

- [Move system databases](https://learn.microsoft.com/en-us/sql/relational-databases/databases/move-system-databases?view=sql-server-ver15)

- [SQL Server Audit (Database Engine)](https://learn.microsoft.com/en-us/sql/relational-databases/security/auditing/sql-server-audit-database-engine?view=sql-server-ver15)

- [Lesson 1: Create and query database objects](https://learn.microsoft.com/en-us/sql/t-sql/lesson-1-creating-database-objects?view=sql-server-ver15)

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

### 🟡 Установка SQL Server Management Studio 20

Перейдите на официальный сайт Microsoft для загрузки [SQL Server Management Studio](https://docs.microsoft.com/en-us/sql/ssms/download-sql-server-management-studio-ssms).

На этой странице найдите ссылку для загрузки последней версии SSMS (Download SQL Server Management Studio (SSMS) 20.2). Скачайте и запустите установленный файл.

![image](https://github.com/user-attachments/assets/e698f33d-3742-41f5-b232-18fed6cc2e6e)

Измените расположение для установки SSMS, если это необходимо, и нажмите Установить.

![image](https://github.com/user-attachments/assets/650e1ee5-31a9-475a-acbd-037531ca87a8)

После установки запустите SQL Server Management Studio из меню Пуск.

![image](https://github.com/user-attachments/assets/2eb3118a-b0c2-45e2-ae46-fc8898af9bdd)

Для соединения с сервером необходимо развернуть контейнеры Docker.

### 🟡 Развёртывание и настройка контейнеров Docker

Создадим директории для каждого докер контейнера на локальной машине, например D:\vm1 и D:\vm2 соответственно.

Выполним команды создания контейнеров в командной строке (для Windows сочетание клавиш Win+R + ввод cmd + enter для открытия командной строки, для открытия командной строки от имени администратора используйте комбинацию ctrl + shift + enter)

![image](https://github.com/user-attachments/assets/e6613ff8-9463-4a7c-b42d-04e8d6d3e3ea)

Скачиваем докер образ:

```powershell
docker pull bsuirmssql/sql-2019:latest
```

Команда запуска первого контейнера:

```powershell
docker run -d `
    --name my_vm1 `
    -e 'ACCEPT_EULA=Y' `
    -e 'MSSQL_SA_PASSWORD=yourPassword123!' `
    -e 'MSSQL_AGENT_ENABLED=true' `
    -p 1433:1433 `
    -p 22:22 `
    -p 5022:5022 `
    -v D:\vm1\Backups:/var/opt/mssql/Backups `
    -v D:\vm1\data:/var/opt/mssql/data `
    -v D:\vm1\Logs:/var/opt/mssql/log `
    -v D:\vm1\data:/var/opt/mssql/ReplData `
    bsuirmssql/sql-2019:latest
```

Команда запуска второго контейнера:

```powershell
docker run -d `
    --name my_vm2 `
    -e 'ACCEPT_EULA=Y' `
    -e 'MSSQL_SA_PASSWORD=yourPassword123!' `
    -e 'MSSQL_AGENT_ENABLED=true' `
    -p 1434:1433 `
    -p 2222:22 `
    -p 5023:5022 `
    -v D:\vm2\Backups:/var/opt/mssql/Backups `
    -v D:\vm2\data:/var/opt/mssql/data `
    -v D:\vm2\Logs:/var/opt/mssql/log `
    -v D:\vm1\data:/var/opt/mssql/ReplData `
    bsuirmssql/sql-2019:latest
```

Выполните команду в командной строке и убедитесь, что созданные вами контейнеры запущены:

```powershell 
docker ps
```

Теперь подключимся к созданному ранее контейнеру my_vm1 и создадим резервную копию базы данных.

Подключение к my_vm1

![image](https://github.com/user-attachments/assets/bf59fa57-a2de-464c-9903-3ab3bbd14d53)

Результат создания backup базы данных testdb

![image](https://github.com/user-attachments/assets/c254ae2d-4f92-4360-9db9-836fd498bac6)

Состояние монтированной папки data после создания backup базы данных testdb

![image](https://github.com/user-attachments/assets/75b759df-effc-403a-b0a1-368ee4ca92e9)

Теперь подключимся ко второму экземпляру из первого, для это при описании докер image мы установили ODBC драйвер и инструменты для работы с SQL Server. Подключимся к первому контейнеру с помощью команды: 

```powershell
docker exec -it my_vm1 bash
``` 

Затем подключимся ко второму экземпляру используя логин и пароль, и порт, на котором находится контейнер.

Результат подключения ко второму контейнеру

![image](https://github.com/user-attachments/assets/ee4b2389-54f4-4069-9191-117344e5f3c8)

А теперь подключимся ко второму контейнеру с локальной машины с помощью SSMS и убедимся, что результат команды совпадаем с полученным ранее.

```sql
SELECT @@SERVERNAME
```

Результат выполнения команды

![image](https://github.com/user-attachments/assets/4e9e5e1f-a28d-4610-90fc-5ec070d84eae)

Результат перенастройки TEMPDB в другое расположение файлов базы данных

![image](https://github.com/user-attachments/assets/bbf136e5-2421-4543-86c7-51ddd2474383)

### 🟡 Создание баз данных

Создадим две базы данных HumanResources и InternetSales. Для каждой базы данных нужно создать несколько файлов данных и журналов. Зададим различные параметры для каждого файла, как указано в задаче.
```sql
CREATE DATABASE HumanResources
ON
PRIMARY (
    NAME = 'HumanResources',
    FILENAME = '/var/opt/mssql/data/HumanResources.mdf',
    SIZE = 50MB,
    FILEGROWTH = 5MB
)
LOG ON (
    NAME = 'HumanResources_log',
    FILENAME = '/var/opt/mssql/log/HumanResources.ldf',
    SIZE = 5MB,
    FILEGROWTH = 1MB
);
CREATE DATABASE InternetSales
ON
PRIMARY (
    NAME = 'InternetSales',
    FILENAME = '/var/opt/mssql/data/InternetSales.mdf',
    SIZE = 5MB,
    FILEGROWTH = 1MB
),
FILEGROUP SalesData1 (
    NAME = 'InternetSales_data1',
    FILENAME = '/var/opt/mssql/data/InternetSales_data1.ndf',
    SIZE = 100MB,
    FILEGROWTH = 10MB
),
FILEGROUP SalesData2 (
    NAME = 'InternetSales_data2',
    FILENAME = '/var/opt/mssql/data/InternetSales_data2.ndf',
    SIZE = 100MB,
    FILEGROWTH = 10MB
)
LOG ON (
    NAME = 'InternetSales_log',
    FILENAME = '/var/opt/mssql/log/InternetSales.ldf',
    SIZE = 2MB,
    FILEGROWTH = 10%
);

ALTER DATABASE InternetSales
MODIFY FILEGROUP SalesData1 DEFAULT;
```

### 🟡 Настройка аудита безопасности

Теперь включим сессию профайлера для отслеживания действий с базой данных и также создадим аудит безопасности, и сделаем настройку безопасности для пользователей и создание ролей.

Главное окно сессии профайлера

![image](https://github.com/user-attachments/assets/4ac37196-07aa-4a82-ad4f-330692149792)

Здесь представлены различные события, в разделе EventClass можно увидеть тип события, например SQL:BatchStarting и SQL:BatchCompleted отслеживает начало и завершение выполнения T-SQL запросов, а Audit Login и Audit Logout — отслеживает вход и выход пользователей.

Также при создании нового профайлера, можно настроить какие события нужно отслеживать.

Выбор отслеживаемых событий профайлера

![image](https://github.com/user-attachments/assets/d3a1621f-0a12-41ab-a198-c37df000cc7b)

Создадим новый аудит безопасности, а также зададим спецификацию и укажем какие типы событий необходимо отслеживать.

Создание аудита безопасности

![image](https://github.com/user-attachments/assets/4e8bd8bb-3ace-47e0-81ab-cacf20bdf613)

Главное окно создания нового аудита

![image](https://github.com/user-attachments/assets/8fb0c0d8-c46f-4542-84af-d50068c052c2)

Создание спецификации аудита сервера

![image](https://github.com/user-attachments/assets/e5e07f68-a1be-4d4a-a9b6-ae4a7a349695)

Для начала работы аудита его необходимо включить.

Включение аудита

![image](https://github.com/user-attachments/assets/8b33cfd1-7194-41fe-b4d8-8431347d8e18)

Создадим группу пользователей с различными ролями и доступом.
Для начала создадим 5 разработчиков с ролями на чтение “db_datareader” и запись “db_datawriter”.

```sql
CREATE LOGIN dev1 WITH PASSWORD = 'DevPassword1';
CREATE LOGIN dev2 WITH PASSWORD = 'DevPassword2';
CREATE LOGIN dev3 WITH PASSWORD = 'DevPassword3';
CREATE LOGIN dev4 WITH PASSWORD = 'DevPassword4';
CREATE LOGIN dev5 WITH PASSWORD = 'DevPassword5';

CREATE USER dev1 FOR LOGIN dev1;
CREATE USER dev2 FOR LOGIN dev2;
CREATE USER dev3 FOR LOGIN dev3;
CREATE USER dev4 FOR LOGIN dev4;
CREATE USER dev5 FOR LOGIN dev5;

ALTER ROLE db_datareader ADD MEMBER dev1;
ALTER ROLE db_datawriter ADD MEMBER dev1;
ALTER ROLE db_datareader ADD MEMBER dev2;
ALTER ROLE db_datawriter ADD MEMBER dev2;
ALTER ROLE db_datareader ADD MEMBER dev3;
ALTER ROLE db_datawriter ADD MEMBER dev4;
ALTER ROLE db_datareader ADD MEMBER dev4;
ALTER ROLE db_datawriter ADD MEMBER dev4;
ALTER ROLE db_datareader ADD MEMBER dev5;
ALTER ROLE db_datawriter ADD MEMBER dev5;
```

Теперь создадим один сервисный аккаунт и назначим ему роли для чтения, запись и обновление записей в таблице UserDB, без доступа к системным базам данных.

```sql
USE master;
CREATE LOGIN app_service WITH PASSWORD = 'AppServicePass';
USE UserDB;
CREATE USER app_service FOR LOGIN app_service;

ALTER ROLE db_datareader ADD MEMBER app_service;
ALTER ROLE db_datawriter ADD MEMBER app_service;
```

Также создадим ещё один сервисный аккаунт для обслуживания и назначим ему роли для модификации базы данных UserDB, создание бэкапов, без удаления БД. Чтение системных баз данных. Ещё один аккаунт для создания бэкапов и 2 аккаунта для тестировщиков. 

Добавление аккаунтов пользователей

![image](https://github.com/user-attachments/assets/6631469a-7d4c-4643-b21a-1cbd97a5f435)

Теперь проверим, что роли, назначенные нами созданным аккаунтам, работают корректно. Результат проверки продемонстрирован.

Проверка ролей разработчика

![image](https://github.com/user-attachments/assets/2aff6719-5080-4850-8a35-6a694a14911a)

Проверка ролей сервисного аккаунта

![image](https://github.com/user-attachments/assets/3aa5b983-da3b-4bc3-bcaa-da6a504bdb62)

Аналогичные настройки пользователей добавим во втором экземпляре контейнера.

### 🟡 Шмфрование, резервирование и перенос данных

Теперь зашифруем базу данных testdb, сделаем её резервную копию и перенесём на второй контейнер и убедимся, что она также зашифрована. Для начала зашифруем testdb в первом контейнере.

Результат шифрования testdb на первой контейнере

![image](https://github.com/user-attachments/assets/a528436a-b0c7-4754-bb31-336d7b6c8c22)

Выполним команды для создания бэкапа, а также сертификата и приватного ключа.

```powershell
BACKUP DATABASE testdb 
TO DISK = '/var/opt/mssql/Backups/testdb_encrypted.bak'
WITH COMPRESSION, FORMAT;
BACKUP CERTIFICATE MyTDECert  
TO FILE = '/var/opt/mssql/Backups/MyTDECert.cer'  
WITH PRIVATE KEY ( 
    FILE = '/var/opt/mssql/Backups/MyTDECert.pvk',    
    ENCRYPTION BY PASSWORD = 'CertPassword123!'
);
```

В результате в директории контейнера ожидаем увидеть три файла.

Файлы бэкапа, сертифкатов и приватного ключа

![image](https://github.com/user-attachments/assets/b2415d53-d55b-44b7-b40f-6a9ea80e14f9)

После подключения к контейнеру номер два (в который хотим перенести сделанный нами ранее бэкап базы данных), выполним в командной строке следующие команды:
- ```powershell
  docker exec -it my_vm2 bash
  ```
> 172.17.0.3 замените на адрес вашего первого контейнера
- ```powershell
  scp root@172.17.0.3:/var/opt/mssql/Backups/testdb_encrypted.bak /var/opt/mssql/Backups/
  ```
- ```powershell
  scp root@172.17.0.3:/var/opt/mssql/Backups/MyTDECert.cer /var/opt/mssql/Backups/
  ```
- ```powershell
  scp root@172.17.0.3:/var/opt/mssql/Backups/MyTDECert.pvk /var/opt/mssql/Backups/
  ```

В командной строке на все вопросы отвечаем утвердительно (yes), для ввода пароля необходимо использовать пароль, который описан в докер образе используемого нами контейнера (в данном примере: yourRootPassword123!).

После этого с помощью утилиты SSMS подключитесь ко второму контейнеру и выполните следующие команды для восстановления базы данных.

```sql
ALTER SERVICE MASTER KEY FORCE REGENERATE;
USE master;
GO
CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'StrongPassword123!';
GO
CREATE CERTIFICATE MyTDECert
FROM FILE = '/var/opt/mssql/Backups/MyTDECert.cer'
WITH PRIVATE KEY (
FILE = '/var/opt/mssql/Backups/MyTDECert.pvk',
 DECRYPTION BY PASSWORD = 'CertPassword123!'
);
GO
GO
RESTORE DATABASE testdb 
FROM DISK = '/var/opt/mssql/Backups/testdb_encrypted.bak'
WITH MOVE 'testdb' TO '/var/opt/mssql/data/testdb.mdf',
MOVE 'testdb_log' TO '/var/opt/mssql/log/testdb.ldf',
REPLACE, RECOVERY;
--Выполним для проверки что база данных также зашифрована
USE master;
SELECT name, is_encrypted FROM sys.databases;
--Убедимся, что таблица не пуста
GO
USE testdb;
SELECT * FROM testTable;
```

Результат просмотра восстановленной и бэкапа таблицы

![image](https://github.com/user-attachments/assets/4824a872-5789-4ba6-868e-81e48e03ccd8)
  
## 🧠 Self-Check Questionnaire

### 🔍 Question 1:
What are the minimum required environment variables to deploy an MS SQL Server container in Docker? Explain the purpose of each.

### 🔍 Question 2:
How would you allow cross-container connections between two SQL Server instances while keeping Windows Firewall active? Provide a T-SQL example for login creation.

### 🔍 Question 3:
Describe the steps to migrate a TDE-encrypted database to another SQL Server instance. Why is certificate backup critical in this process?

### 🔍 Question 4:
What events would you track in a SQL Server Audit to monitor unauthorized access attempts? Name 3 key event classes.

### 🔍 Question 5:
What is the difference between db_datareader and db_datawriter roles? Give a scenario where assigning both would be insecure.
