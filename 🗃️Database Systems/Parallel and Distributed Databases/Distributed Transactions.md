The problem with distributed transactions is if a few machines commit while another machine crashes. We need to maintain the same [[Transactions and Serializability#^8dd786|atomicity]] guarantees in the distributed setup.

## Two-Phase Commit

We add a new state called PREPARED to transactions. This indicates that the node has done the work for a transaction, and the decision to COMMIT/ABORT will be done by a *coordinator*.

Once prepared, a node must not COMMIT or ABORT on its own. This PREPARED state must survive crashes.

Here is what the coordinator does:

1. Log start of transaction.
2. Execute transaction on worker nodes.
3. PREPARE each worker.
4. Once workers are all prepared, log transaction commit.
5. COMMIT each worker.
6. Log DONE, so we know all transactions are done.

Note that step 4 is considered the commit point, and once it reaches this point the system is forced to follow through with the transaction and make it durable.

If the coordinator crashes, we can read off the log.

* If it is before the coordinator commit, ABORT all workers.
* If it is after the coordinator commit, but not done, COMMIT all workers.

> [!warning]
> This is resistant to all failure cases as long as the coordinator/worker recovers from the crash quickly enough. It cannot handle cases where, e.g. a worker is gone for good.

Another method is called ==presumed abort==, where if you don't explicitly see a COMMIT in the logs (or from the coordinator) you just ABORT on recovery.

Some of the issues with 2PC are:

1. You have high overheads from 2 network round trips and synchronous logging.
2. If coordinator fails, the workers must wait on it to come back.
3. If coordinator and a worker fails, there is no way to recover.

2PC generally sacrifices the availability of a system for its consistency.
