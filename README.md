# cse716 - ADVANCED DATABASE SYSTEMS

## Topic
- Replicated Concurrency Control and Re-covery (RepCRec for short)

## Group members
- Name: Iftekhar Hossain, Id: 19366008
- Name: Shareea Sabreen, Id: 21366004
- Name: Samiha Sultana, Id: 21166048
- Name: Sanjida Ali Shusmita, Id: 21366018
- Name: Shiva Bahadur Pathak, Id: 21166052

## PROGRAMMING LANGUAGE
- Java

## Project Description
- Implement a distributed database with multiversion concurrency control, deadlock detection, replication, and failure recovery. 
- deep insight of the design of a distributed system. 
- compare/evaluate strategy for concurrency, deadlock detection and failure recovery with existing other systems.

## Build the project using Gradle
- command: gradlew clean build
- It creates a directory 'build' in project directory
 
## Run the program using Gradle
- command: gradlew run

## Run the program from build
- You will get a jar file named "RepCRec-1.0-SNAPSHOT.jar" in build/libs directory 
- command: java -jar RepCRec-1.0-SNAPSHOT.jar

## ALGORITHMS USED

### Strict Two Phase Locking
- We use strict two phase locking (using read and write locks) at each site and validation at commit time.

### DFS to detect deadlock detection
- We use DFS to detect deadlock cycle and abort the youngest transaction.

### Mulitversion Concurrency Control
- Read only transaction acquires a snapshot of the database when the transaction begins. All values that have been committed at that time point will be recorded by the transaction. If an item is not available, the transaction will wait and acquire the snapshot later when the site recovers.

### Site Failure and Site Recovery
- When a site fails, its lock table is wiped out.
- When a site recovers, all the unreplicated items are immediately available for reads and writes. Replicated items need to wait for a committed write.

## COMPONENTS
- Application: Entry point of the program.
- IOUtils: Sends instructions to Transaction Manager by going though the input file line by line.
- Transaction Manager: Transaction Manager executes all operations. It manages failing sites, recovering sites, making changes to data items' values, figuring out where the deadlocks are, deciding whether to commit or abort a transaction. It acts as middleman for the Transaction Manager and the 10 sites.
- Transaction: A unit of work that has a number of operations performed within the database.
- Lock Manager: Lock Manager is responsible for acquiring locks, releasing locks.
- Data Manager: Insert, update, commit values.
- Site: Manages lock and data of Site. Each site has a data table and a lock table. It receives directions from the Transaction Manager regarding failing and recovering of a site. 
- Transaction status: Enum of different site status.
- Operation: BEGIN, END, BEGIN_RO, READ, WRITE, DUMP, FAIL, RECOVER are different operations.