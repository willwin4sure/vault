> Do not communicate by sharing memory; instead, share memory by communicating.
> 
> — Golang documentation

Rust's standard library provides an implementation of ==channels== for message-passing. Each channel has a ==transmitter== and a ==receiver==.

Here's the basic syntax:

```rust
use std::sync::mpsc;

fn main() {
	let (tx, rx) = mpsc::channel();  // doesn't compile yet
}
```

Here, `mpsc` stands for ==multiple producer, single consumer==, which is what it sounds like. `let` with a pattern destructures the tuple, and we get the transmitter `tx` and the receiver `rx`. 

We can move the transmitter into a thread and send a message. Then we can listen on the other end.

```rust
use std::sync::mpsc;
use std::thread;

fn main() {
	let (tx, rx) = mpsc::channel();

	thread::spawn(move || {
		let val = String::from("hi");
		tx.send(val).unwrap();
	});

	let received = rx.recv().unwrap();
	println!("Got: {received}");
}
```

Note that the `send` method returns a `Result` type in case the receive has already been dropped and we need to raise a `SendError`. We call `unwrap` here to just panic in that case.

The `recv` method blocks (like in other languages) until a value is sent down the channel. If the transmitter closes, then `recv` returns an error in its `Result`. There is also a `try_recv` message that returns a `Result` immediately, that is `Ok` if a message is there and `Err` otherwise.

## Channels and Ownership Transference

Recall that these threads are sharing a virtual address space, and any heap-allocated memory is visible to both. So the concept of ownership is extremely useful for presenting bugs.

For example, after we send `val` across the channel, we wouldn't want the transmitter thread to access `val` afterwards, since the other thread could easily drop it, for instance. This is beautifully encoded in the fact that `val`'s ownership is taken on by the `send` method (later, the receiver will take ownership).

## Multiple Messages

Here's an example of sending multiple messages:

```rust
use std::sync::mpsc;
use std::thread;
use std::time::Duration;

fn main() {
    let (tx, rx) = mpsc::channel();

    thread::spawn(move || {
        let vals = vec![
            String::from("hi"),
            String::from("from"),
            String::from("the"),
            String::from("thread"),
        ];

        for val in vals {
            tx.send(val).unwrap();
            thread::sleep(Duration::from_secs(1));
        }
    });

    for received in rx {
        println!("Got: {received}");
    }
}
```

You can really feel the delay between each message send. Further, we aren't calling the `recv` method explicitly anymore. Instead, we are treating `rx` as an iterator. This operates on each received value, and terminates when the channel is closed.

## Multiple Producers

Let's use the multiple producer part of `mpsc` to use. We can do this by cloning the transmitter and sending them to different threads, e.g.:

```rust
    // --snip--

    let (tx, rx) = mpsc::channel();

    let tx1 = tx.clone();
    thread::spawn(move || {
        let vals = vec![
            String::from("hi"),
            String::from("from"),
            String::from("the"),
            String::from("thread"),
        ];

        for val in vals {
            tx1.send(val).unwrap();
            thread::sleep(Duration::from_secs(1));
        }
    });

    thread::spawn(move || {
        let vals = vec![
            String::from("more"),
            String::from("messages"),
            String::from("for"),
            String::from("you"),
        ];

        for val in vals {
            tx.send(val).unwrap();
            thread::sleep(Duration::from_secs(1));
        }
    });

    for received in rx {
        println!("Got: {received}");
    }

    // --snip--
```

You will probably see these messages interleaved in a nondeterministic way.

---

**Next:** [[Shared-State Concurrency]]