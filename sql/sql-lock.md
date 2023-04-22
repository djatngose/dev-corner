# Shared Lock (SQL Server) = Read Lock (ReaderWriterLockSlim)
A shared lock allows multiple transactions to read a specific row or page in the database but prevents any of them from modifying the data until the lock is released. Here's an example of using a shared lock in SQL Server:
```c#
BEGIN TRANSACTION;
SELECT * FROM myTable WITH (TABLOCKX, HOLDLOCK);
-- Do some read-only operations
COMMIT TRANSACTION;

```
n `ReaderWriterLockSlim`, a read lock allows multiple threads to read a shared resource but prevents any of them from writing to it until the lock is released:

```c#
_readerWriterLockSlim.EnterReadLock();
// Do some read-only operations
_readerWriterLockSlim.ExitReadLock();
```

# Exclusive Lock (SQL Server) = Write Lock (ReaderWriterLockSlim)
An exclusive lock allows a transaction to modify a specific row or page in the database while preventing other transactions from reading or modifying the data until the lock is released. Here's an example of using an exclusive lock in SQL Server:
```c#
BEGIN TRANSACTION;
UPDATE myTable SET col1 = 'new value' WHERE id = 1;
-- Do some more operations
COMMIT TRANSACTION;

```

In `ReaderWriterLockSlim`, a write lock allows a single thread to modify a shared resource while preventing other threads from reading or writing to it until the lock is released:
```c#
_readerWriterLockSlim.EnterWriteLock();
// Do some write operations
_readerWriterLockSlim.ExitWriteLock();

```

# Update Lock (SQL Server) = Upgradeable Lock (ReaderWriterLockSlim)
An update lock allows a transaction to read a specific row or page in the database with the intention of modifying it later, while preventing other transactions from modifying the data. Here's an example of using an update lock in SQL Server:
```c#
BEGIN TRANSACTION;
SELECT * FROM myTable WITH (UPDLOCK, HOLDLOCK) WHERE id = 1;
-- Do some more operations
COMMIT TRANSACTION;

```

In `ReaderWriterLockSlim`, an upgradeable lock allows a thread to read a shared resource with the intention of upgrading the lock to a write lock later. While the lock is upgradeable, other threads can still read the shared resource but not write to it:

```c#
_readerWriterLockSlim.EnterUpgradeableReadLock();
// Do some read-only operations
if (needToWrite) {
    _readerWriterLockSlim.EnterWriteLock(); // Upgrade the lock to a write lock
    // Do some write operations
    _readerWriterLockSlim.ExitWriteLock();
}
_readerWriterLockSlim.ExitUpgradeableReadLock();

```


Here's an example scenario: Let's say we have a table called Employees with the following columns: EmployeeID, FirstName, LastName, and Salary. We want to increase the salary of a particular employee by 10%, but we also want to prevent other transactions from modifying the same employee's salary while the update is in progress.

To do this, we can use an update lock. Here's some sample T-SQL code that demonstrates how to use an update lock:
```sql
BEGIN TRANSACTION

-- Acquire an update lock on the employee record
SELECT * FROM Employees WITH (UPDLOCK)
WHERE EmployeeID = 12345

-- Perform the salary update
UPDATE Employees
SET Salary = Salary * 1.1
WHERE EmployeeID = 12345

COMMIT TRANSACTION
```
In this example, the WITH (UPDLOCK) hint tells SQL Server to acquire an update lock on the Employees table when performing the SELECT statement. This update lock prevents other transactions from acquiring exclusive locks on the same employee record, which would prevent the salary update from succeeding.

Once the update lock is acquired, we can safely update the employee's salary knowing that no other transaction is modifying the same record. Finally, we commit the transaction to release the update lock and allow other transactions to access the updated data.