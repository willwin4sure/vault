We will talk about this in the context of Golang, the programming language used for labs.

> [!question]
> Why Go?

1. Good support for threads and RPC.
2. Garbage collected, which is very useful especially when dealing with shared memory.
3. Type-safe.
4. Simple.
5. Compiled.

## Threads

A ==thread== is a sequence of instructions essentially executed as its own sequential program, i.e. it has its own program counter, stack, and registers. Threads may share memory with other threads, which enables efficient parallelism.

In Go, you can start a thread by calling `go`. They actually run in parallel, unlike in other programming models (e.g. [[Async Await]] in JavaScript). Threads exit when they are done with work. They can also be interrupted, i.e. stopped when blocked and resumed later with reloaded state.

> [!question]
> Why threads?

They express concurrency, allowing:

* I/O concurrency, hiding latency.
* Multicore parallelism.
* Convenience (e.g. repetitive background activities).

In Go, you should think of threads as rather lightweight. They aren't free but make as many as you need!

However, there are many challenges with threads, the main of which are ==race conditions==. You've seen these in [[Races and Parallelism|performance engineering]]. Here are some solutions:

* Protecting shared memory with locks. Go has a race detector.
* Don't share data at all! Share via communication through Go channels (message passing).

Another issue is ==coordination==. You can solve this with channels or condition variables. Finally, you might have ==deadlocks==.

## Go Threads

Go has some nice primitives for dealing with this that you can learn in the [Tour of Go](https://go.dev/tour/concurrency/1). One thing not discussed are condition variables. Here's an example:

```go
func main() {
	count := 0
	finished := 0
	var mu sync.Mutex
	cond := sync.NewCond(&mu) // cond var associated with mutex

	for i := 0; i < 10; i++ {
		go func() {
			vote := requestVote() // expensive op that returns bool
			mu.Lock()
			defer mu.Unlock()
			if vote {
				count++ 
			}
			finished++
			cond.Broadcast() // wakes all threads waiting on cond var.
			                 // there is also cond.Signal() that wakes
			                 // one thread waiting on the cond var.
		}()
	}

	mu.Lock()
	for count < 5 && finished != 10 {
		cond.Wait() // unlocks mutex and blocks thread until signalled
		            // automatically reacquires lock when awoken
	}

	if count >= 5 {
		println("received 5+ votes!")
	} else {
		println("lost")
	}
}
```

## Remote Procedure Calls

The goal is for these to behave as similarly to normal procedure calls as possible. A client issues a request for a function to run, and the server does that computation and returns the result.

When the client initiates an RPC, it calls a ==stub==, which is a local function that ==marshals== the function and arguments into a message to pass to the server. The server-side stub ==unmarshals== the bytes of the message and actually calls the function, and returns the value to the stub. Then the client stub, which is waiting, unmarshals the return value and gives it to the client.

There are a few different RPC semantics under server failures:

1. **At Least Once**. Client automatically retries. May cause duplicates.
2. **At Most Once**. The server filters out duplicates.
3. **Exactly Once**. Hard! You'll need to write stuff to disk, and most RPCs don't implement this.

Failures are what expose the differences between RPCs and PCs.

## Go RPCs

Here's an example of the Get part of a simple key/value store.

```go
//
// Common
//

const (
	OK       = "OK"
	ErrNoKey = "ErrNoKey"
)

type Err string

type GetArgs struct {
	Key string
}

type GetReply struct {
	Err   Err
	Value string
}

//
// Client
//

func connect() *rpc.Client {
	client, err := rpc.Dial("tpc", ":1234")
	if err != nil {
		log.Fatal("dialing:", err)
	}
	return client
}

func get(key string) string {
	client := connect()
	args := GetArgs{"subject"}
	reply := GetReply{}
	err := client.Call("KV.Get", &args, &reply)
	if err != nil {
		log.Fatal("error:", err)
	}
	client.Close()
	return reply.Value
}

//
// Server
//

type KV struct {
	mu   sync.Mutex
	data map[string]string
}

func server() {
	kv = new(KV)
	kv.data = map[string]string{}
	rpcs = rpc.NewServer()
	rpcs.Register(kv)
	l, e = net.Listen("tpc", ":1234")
	if e != nil {
		log.Fatal("listening:", err)
	}
	go func() {
		for {
			conn, err := l.Accept()
			if err == nil {
				go rpcs.ServeConn(conn)
			} else {
				break
			}
		}
		l.Close()
	}()
}

func (kv *KV) Get(args *GetArgs, reply *GetReply) error {
	kv.mu.Lock()
	defer kv.mu.Unlock()

	val, ok = kv.data[args.Key]
	if ok {
		reply.Err = OK
		reply.Value = val
	} else {
		reply.Err = ErrNoKey
		reply.Value = ""
	}
	return nil
}
```

---

**Next:** [[Primary-Backup Replication]]