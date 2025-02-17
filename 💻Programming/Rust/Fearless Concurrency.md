By leveraging ownership and type checking, Rust will refuse to compile a lot of incorrect code, so you'll have less headaches reproducing bugs at runtime. This is called ==fearless concurrency==.

However, as a language supporting performant applications, Rust cannot give up control of the hardware for simpler, safer abstractions. It has tools for both message-passing and shared-state concurrency.

1. [[Threads]]
2. [[Message Passing]]
3. [[Shared-State Concurrency]]
4. [[Sync and Send Traits]] ^d5b601

---

**Next:** [[OOP in Rust]]