# Size Matters

## Why does size matter?
* You have data
* It will grow larger
* Performance may suffer
* You want to make things better

## How database designs fail
* Many designs do not account for size
* Row counts might be considered
* What about inside the rows?
  * The width of the columns is often overlooked
* Great database performance starts with great database design
 
## Datatype Pain #1: Disk Space
* Disk is cheap...but there are hidden costs!
  * More storage = longer maintenance
  * More storage = longer database backups
  * More storage = longer offsite backups
* Most don't think about how much space is used by that tiny piece of data stored multiple times

## Datatype Pain #2: Memory Space
* Those extra bytes are read from disk into SQL Server buffer cache
* Extra logical I/O needed to return query results
* What about indexes?
  * Same thing, they drag around extra I/O
  
## Datatype Pain #3: Network
* Extra bandwidth to move the data around

## Verify Datatypes
* Compare datatype definition to actual data
  * i.e. INT defined (4 bytes), but only TINYINT used (1 byte)
* Can estimate savings
  * Disk space as well as logical I/O
* Can be intrusive to alter existing data
  * Especially if PK/FK defined
  
## Real life example
* PaymentAmount lookup table
  * PaymentAmountId INT
* PaymentAmountId is referenced in 9 tables
* The PaymentAmount table has been in the production for 8 years
* In that time, we only added 17 rows to this table so TINYINT probably would have been a safe bet

* DataTable1
  * rows: 217,721,308
  * data: 26.7 GB
  * 9 indexes (this field is in two indexes): 48.6 GB
  * 3 bytes * 3 places (table, 2 indexes) * 217721308 rows = 1.96 GB in potential savings
  
* DataTable2
  * rows: 162,988,609
  * data: 51.2 GB
  * 2 indexes: 10.6 GB
  * 3 bytes * 3 places (3 columns in the table) * 162988609 rows = 1.47 GB in potential savings
  
* DataTable3
  * rows: 267,760,350
  * data: 39.4 GB
  * no index other than clustered primary key
  * 3 bytes * 1 place * 267760350 rows = 0.80 GB in potential savings
  
## Steps for right sizing
* Understand the storage requirements for every datatype you consider
* Review all design decisions based on the shape of the data -- where it is now and where it is likely to be later
* Set datatypes based on business requirements not tool defaults
* Measure and monitor fit of the data to its datatypes
