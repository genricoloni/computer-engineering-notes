# Basic building blocks in Fault tolerant distributed systems

## Atomic actions

We intend as atomic an action that is indivisible, that is, it is executed in its entirety or not at all. In a distributed system, an atomic action is generally executed at more than one node, so these nodes have to cooperate in order to guarantee the atomicity property: if one of the nodes fails, the action shouldn't let its effects be visible to the entire system.

### Atomic actions in a database

In a database, an atomic action is a transaction, that can be composed by more than one operation such as read, write, update or deleted a record in the database.

#### Commit

When a transaction is committed, all its effects are made visible to the entire system. This means that the transaction is finished and its effects are permanent. An example of a transaction is a bank transfer, where the money is removed from one account and added to another account, as we can see in the following example:

```sql
t: begin transaction

    UPDATE account
    SET balance = balance + 500
    WHERE account_number = 45;

    UPDATE account
    SET balance = balance - 500
    WHERE account_number = 35;

t: commit
end transaction
```

#### Abort or Rollback

When a transaction is aborted, all its effects are discarded and the system is restored to the state it was before the transaction started. An example of a transaction that is aborted is a bank transfer where the money is added to one account but not removed from the other account because of balance insufficiency, as we can see in the following example:

```sql
t: begin transaction

    UPDATE account
    SET balance = balance + 500
    WHERE account_number = 45;

    UPDATE account
    SET balance = balance - 500
    WHERE account_number = 35;

    SELECT balance
    INTO V FROM account
    WHERE account_number = 35;

    IF V >=0 THEN
        commit;
    ELSE
        abort;
    END IF;
;
end transaction
```

### Atomic actions in a distributed system

Imagine now that the bank database is distributed, and the two accounts are stored in two different nodes. In this case, the transaction has to use data stored in different nodes: a possible rollback of the transaction must leave both nodes in a consistent state, that is, the state they were in before the transaction started. Using the previous codes as an example, the DBMS has to guarantee that if the transaction is aborted, the money is not added to the first account and the money is not removed from the second account, considering the fact that these information are stored in different nodes.

### Two-phase commit

The two-phase commit is a protocol that guarantees that a transaction is either committed or aborted in a distributed system. We have some crucial elements to consider:

- the **transaction manager TM**, that is the entity that coordinates the transaction;
- some **resource managers RM**, that are the entities that manage the resources involved in the transaction.
- a **log file**, stored in persistent memory, that is used to recover the state of the transaction in case of failure;
- a **timeout**, that is the maximum time that the TM waits for a response from the RMs.

A scheme of the two-phase commit is the following:

![Two-phase commit](../images/03/twoPhase.png){ width=400px}

In case of fail of the TM, we have an **uncertain period**, during which a RM with a ready state cannot terminate the transaction. Note that this mechanism is able to tolerates both loss of messages and crash of nodes, simply rolling back the transaction.

### Three-phase commit

This protocol is an evolution of the previous one, where a **pre commit** phase is added. Assuming a permanent crash of the TM, a participant is able to substitute the coordinate, in order to terminate the transaction, with a **global commit** or a **global abort**.

![Three-phase commit](../images/03/threePhase.png){ width=400px}

### Recovery and atomicity

Given a block $B$, that can be a **physical block** (a disk block) or a **buffer block** (temporary storage in memory), the operations to move a block from a disk to a buffer are **input(B)** and **output(B)**. A transaction $T_i$ has its own private work-area, in which local copies of all accessed items are stored, performing the operations **read_item(X)** and **write_item(X)**. The system can perform the **output** of the operation, trough the **output($B_X$)** operation, that need not immediately follows the **write_item(X)** operation.

In general, several outputs are required by a transaction, and the latter can be aborted after one of these operations have been made permanent; it's also true that a transaction can be committed, and a failure can occur before all the outputs are made permanent. In order to guarantee the atomicity property, the system has to perform the **output** operation in a way that the transaction is either committed or aborted, and the **output** operation is either made permanent or not, using a **log file** to recover the state of the transaction in case of failure.

### Log file

The log file is a file stored in persistent memory, that is used to recover the state of the transaction in case of failure. Before the commit of a transaction, system has to save into the log file all the records of the operations that have been made permanent, and after the commit of a transaction, system has to save into the log file a record that indicates the commit of the transaction, executing **UNDO** and **REDO** operations in case of failure, to restore the state of the transaction.

## Consensus problem