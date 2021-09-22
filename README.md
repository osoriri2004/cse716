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

### Available Copies Algorithm

### Strict Two Phase Locking

### Tarjan’s Algorithm for deadlock detection

### Mulitversion Concurrency Control
- Read only transaction acquires a snapshot of the database when the transaction begins. All values that have been committed at that time point will be recorded by the transaction. If an item is not available, the transaction will wait and acquire the snapshot later when the site recovers.

### Site Failure and Site Recovery
- When a site fails, its lock table is wiped out.
- When a site recovers, all the unreplicated items are immediately available for reads and writes. Replicated items need to wait for a committed write.

### Available Copies Algorithm
- Returns the first available copy. If the item is replicated, the Site Manager will start looking from Site 1. If the item is not available in any site, the transaction will wait.
- At commit time, the Site Manager determines whether a transaction commits based on the timestamp of each of its operations and the timestamp of the last failure from the sites the transaction has touched. If there is a site that has failed, the transaction will abort, otherwise, it will commit.
- If a transaction is blocked because it cannot acquire lock or a site has failed, it will resume once the item is available or the site is recovered.

### Strict Two Phase Locking
- The Site Manager will determine whether a transaction can acquire a lock. All locks are released at the end of a transaction.
- For shared locks, a transaction can obtain lock for an item if no other transaction is waiting for the item.
- For exclusive locks, a transaction can obtain lock if no other transaction is reading or writing the item.

### Deadlock Detection
- The Site Manager looks for deadlock every time a new line is read from the input file. It uses the Tarjan’s Algorithm to find strongly connected components in the wait for graph
- The Site Manager returns any cycle found to the Transaction Manager, and the Transaction Manager kills the youngest.

### Mulitversion Concurrency Control
- Read only transaction acquires a snapshot of the database when the transaction begins. All values that have been committed at that time point will be recorded by the transaction. If an item is not available, the transaction will wait and acquire the snapshot later when the site recovers.

### Site Failure and Site Recovery
- When a site fails, its lock table is wiped out.
- When a site recovers, all the unreplicated items are immediately available for reads and writes. Replicated items need to wait for a committed write.

## COMPONENTS

1. Application: Entry point of the program.

2. IOUtils: Sends instructions to Transaction Manager by going though the input file line by line.

3. Transaction Manager: Transaction Manager executes all operations. It manages failing sites, recovering sites, making changes to data items' values, figuring out where the deadlocks are, deciding whether to commit or abort a transaction. It acts as middleman for the Transaction Manager and the 10 sites.

4. Transaction: A unit of work that has a number of operations performed within the database.

5. Lock Manager: Lock Manager is responsible for acquiring locks, releasing locks.

6. Data Manager: Insert, update, commit values.

7. Site Manager: Manages lock and data of Site. Each site has a data table and a lock table. It receives directions from the Transaction Manager regarding failing and recovering of a site. 

8. Transaction status: Enum of different site status.

9. Operation: BEGIN, END, BEGIN_RO, READ, WRITE, DUMP, FAIL, RECOVER are different operations.