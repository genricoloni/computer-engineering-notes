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

The consensus problem describes **how a set of distributed systems can agree on a value**, despite failures; to help its description, the *Byzantines Generals* metaphor is used. In this scenario, generals must agree on an attack plan or a retreat, but not all of them are loyal: we have the presence of traitors among them, who may lie in order to influence the plan to take an advantage to the enemies. Each general observes the enemy abd communicates their observations to the others: traitors may lie in order to support their own plan, or even misrepresent the observations of the loyal generals, making it difficult to reach an agreement.

### The Byzantine Generals problem

As we just said, generals can be loyal or traitors, and they reach the consensus when:

1. all loyal generals agree on the same plan;
2. a small number of traitors cannot cause the loyal generals to adopt a bad plan.

#### Assumptions

The general assumptions of the Byzantine Generals problem are:

- let $n$ be the number of generals;
- the opinion of a general is either **attack** or **retreat**, described by the function $v(i)$;
- each general $i$ share their value $v(i)$ to each other general;
- each general final decision is obtained by a majority vote among the opinions shared by the generals.

#### Role of the traitors

We have **absence of traitors** when the generals have the same value $v(1), v(2), ..., v(n)$, a situation that results in taking the same decision. On the contrary we have **presence of traitors** when the generals have different values, and the traitors can influence the decision of the loyal generals. In particular, in presence of traitors:

- to satisfy the first condition, the loyal generals have to agree on the same value. In other words, every general must apply the majority function to the same set of values $v(1), v(2), ..., v(n)$;
- to satisfy the second condition, for each general $i$, if they are loyal, the the value they $v(i)$ they sent must be used by every loyal general as the value of general $i$.

#### Interactive consistency

Interactive consistency is a concept where a commanding general $C$ and $n-1$ lieutenants $L_1, ... L_{n-1}$ must agree of a action plan. Then, we have two situations:

- *$IC_1$*: all loyal lieutenants obey the order of the commanding general;
- *$IC_2$*: the decision of loyal lieutenants must be the same as the decision of the commanding general, if they're loyal.

In simple terms, the Interactive consistency ensures that all loyal lieutenants follow the same command issued by the general, maintaining consistency in the decision.

#### Case study: 3 generals and 1 lieutenant traitor

FIrst of all, note that **there are no solutions** for this case. Let's now observe the scheme of the generals:

![Byzantine Generals](../images/03/byzantine1.png){ width=400px}

In this situation, whit two different commands, we assume that $L_1$ must obey the command of $C$:

- if $L_1$ decides to attack, both $IC_1$ and $IC_2$ are satisfied;
- if $L_1$ decides to retreat, $IC_1$ is satisfied, but $IC_2$ is not.

From the latter, we can get the following rule: **if $L_i$ receives different commands, they will always takes the decision received by $C$**.

#### Case study: 3 generals and 1 general traitor

The situation is the same as the previous one, and the same inferred rule is applied:

![Byzantine Generals](../images/03/byzantine2.png){ width=400px}

- $L_1$ must obey to $G$, and decides to attack;
- $L_2$ must obey to $G$, and decides to retreat.

In this case, the $IC_1$ is violated, because the lieutenants have different decisions, but the $IC_2$ is satisfied. In general, **four generals are required to cope with one traitor**.

#### Generalization of the Byzantine Generals problem

A general scheme for the Byzantine Problem is the following:

![Byzantine Generals](../images/03/byzantine3.png){ width=400px}

In case there isn't any traitor, each $L_i$ will receives three copies of the same order they've already received from $C$.

### Oral message algorithm

In this section, we'll go through the oral message algorithm, that is a solution to the consensus problem. The algorithm is based on the following assumptions:

- the system is **synchronous**;
- any two processes can communicate with each other, and the **communication is reliable and instantaneous**;
- the **receiver** can identify the **sender** of the message;
- every message sent by a non-faulty process is **eventually delivered**;
- the **absence of messages** can be detected.

This algorithm is able to solves the Byzantine problem for $n \ge (3m+1)$ general, and includes a majority vote on the values received from the lieutenants: note that, if a traitor doesn't send any message, we assume that the message is *retreat*; consider also the fact that the majority vote returns *retreat* if a majority is not reached.

#### The algorithm for $OM(0)$

1. The commander C sends their value to every lieutenant $L_i$, with $i \in \{1, ..., n-1\}$;
2. Each lieutenant eses the received value.

#### The algorithm for $OM(m)$, with $m>0$

1. $C$ sends their value to every lieutenant $L_i$, with $i \in \{1, ..., n-1\}$;
2. Let $v_i$ the value received by $L_i$ from $C$: $L_i$ acts as $C$ in $OM(m-1)$ to send $V_i$ to each of the $n-2$ lieutenants;
3. For each $i$, with $j \ne i$, let $v_j$ be the value that goes from $L_j$ to $L_i$ in the previous step;
4. Perform a majority vote among values $\{v_1, ..., v_{n-1}\}$

The algorithm is **recursive**, and it invokes $n-1$ calls for $OM(m-1)$, $n-2$ calls for $OM(m-2)$ and so on.

### Consideration about the algorithm

Solution for consensus problem are highly costly, requiring a minimum of $3m+1$ nodes and $m+1$ rounds, with message sizes $O(n^{m+1})$ growing at each round. Different metrics are used to evaluate algorithms, including the number of faulty processors, the number of rounds, and the message size; some of these algorithms have been proven optimal for specific aspects.

### Signed messages

The problem is made challenging due to the ability of the traitors to lie: in order to address this aspect, a solution involving signed messages has been proposed, enabling generals to send unforgeable and authenticated messages. This is a huge simplification of the problem, that relies on the following assumptions:

- the **signature cannot be forged**, and any alteration of a signed message can be detected;
- the authenticity verification can be made by everyone.

However, no assumptions on the signature of traitor generals have been made.

#### The choice algorithm

Let $V$ be a set of orders, with a function `choice(V)` that obtain a single order from a set. The function will returns:

- *retreat* if $V$ is empty;
- $v$ if $V$ is composed only by a single element $v$;
- *retreat* if $V$ consists of more than one element.

General $0$ is the commander, and for each $i$, $V_i$ is the set of properly signed orders that Lieutenants $L_i$ received so far.

#### Signed algorithm $SM(m)$

The algorithm ensures that at most $m$ lieutenants can send an order to their subordinates. The algorithm is based on the following steps:

1. at the start, all lieutenants have an empty set $V_i$;
2. $C$ signs and sends their value to every lieutenant $L_i$, except for the last one;
3. each $L_i$ collects and verifies the received signed messages, appending their signatures if necessary, and broadcast the updated message to other lieutenants;
4. when no more messages are received, each lieutenant applies the `choice` function to the set of received messages.

Some key observations about the algorithm:

- lieutenants will discard any messages that have been already received;
- if $L_I$ is the $m$-th lieutenant that adds their signature to the message, the message is not broadcasted anymore;
- time-outs techniques can be used to detect the absence of messages.

There is an algorithm, that has been formally proved, that states: **for any $m$, the algorithm $SM(m)$ solves the Byzantine Generals problem for $n \ge 2m+1$ generals and m traitors**.

### Impossibility results

In **asynchronous distributed systems**, where no timing assumptions are made, making them easier to port application and handle variable workloads, the consensus problem **cannot be deterministically solved**, and this because it's difficult to distinguish between a slow process and a failed one, and deciding to stop  a single process in a inopportune moment can lead to a failure of the consensus algorithm. In order to circumvent this issue, different strategies can be used, such as:

- **loosely synchronized systems**, where different processors allocated to a task are executing the same iteration, so they don't need tight synchronization;
- **median clock algorithm**; each clock observes every other clocks, and sets its time to the median value of the observed clocks.

### CLock synchronization

Let's observe the following figure:

![Clock synchronization](../images/03/clocksyn.png){ width=400px}

Without loss of generality, we can state that clock($A$) $ < $ clock($B$) $, and assume $C$ as faulty. Assume that $A$ and $B$ observe the same value of C: we have a **non Byzantine failure**, with processes that may obtain different values from the faulty process.

Another case can be shown with the following hypothesis:

- clock($A$) $= 10  < $ clock($B$) $ = 20$;
- assume a Byzantine failure of $C$.

If $A$ sees a values for clock $C$ that is slightly lower than its own value, and $B$ sees a value for clock $C$ that is slightly higher than its own value, both $A$ and $B$ will see their own value as the median value, and the algorithm will fail, and this because **the median computed by two different processes will be the same if the set of clock values they obtain is the same**.