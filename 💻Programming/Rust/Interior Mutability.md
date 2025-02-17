One issue with `Rc<T>` from the [[Reference Counted Smart Pointer|last section]] is that you can't share mutable references in this way, since that could violate the rules for references. In this section, we'll talk about how to fix this with `RefCell<T>` in conjunction with `Rf<T>`.

In Rust, ==interior mutability== is a design pattern that lets you mutate data even when there are immutable references to that data. Normally, this is disallowed by the [[References and Borrowing#^e94134|rules of references]]. 

> [!idea]
> Think about having `const` objects in C++ but using the `mutable` keyword for some internal state.

In order to mutate data, we use [[Unsafe Rust|unsafe]] code to bend Rust's usual rules: we are indicating that we will check rules manually, rather than relying on Rust's compiler (we can guarantee more things). This unsafe code will be wrapped in a safe API, and the outer type is still immutable.

## Runtime Borrow Checking with `RefCell<T>`

`RefCell<T>`'s are basically `Box<T>` objects, with one difference: if you violate the rules of references with references or [[Boxes for Heap Allocation|boxes]], your code will not compile. If you break the rules with a `RefCell<T>`, your code will panic and exit at runtime.

This allows you more flexibility, as the Rust compiler is inherently conservative with what it allows to compile and may reject a correct program.

> [!warning]
> Like `Rc<T>`, `RefCell<T>` should only be used in single-threaded scenarios!

## Application: Mock Objects

These are one use case for interior mutability, mock objects for testing purposes. Here's some code you should be able to read.

```rust
pub trait Messenger {
    fn send(&self, msg: &str);
}

pub struct LimitTracker<'a, T: Messenger> {
    messenger: &'a T,
    value: usize,
    max: usize,
}

impl<'a, T> LimitTracker<'a, T>
where
    T: Messenger,
{
    pub fn new(messenger: &'a T, max: usize) -> LimitTracker<'a, T> {
        LimitTracker {
            messenger,
            value: 0,
            max,
        }
    }

    pub fn set_value(&mut self, value: usize) {
        self.value = value;

        let percentage_of_max = self.value as f64 / self.max as f64;

        if percentage_of_max >= 1.0 {
            self.messenger.send("Error: You are over your quota!");
        } else if percentage_of_max >= 0.9 {
            self.messenger
                .send("Urgent warning: You've used up over 90% of your quota!");
        } else if percentage_of_max >= 0.75 {
            self.messenger
                .send("Warning: You've used up over 75% of your quota!");
        }
    }
}
```

This is tracking your compute budget, say. The important thing is that the `Messenger` trait takes an immutable reference to `self` and a message. Let's say you want to use a `MockMessenger` when testing `set_value`. All we want it to do is keep track of the messages that have been sent so far.

```rust
#[cfg(test)]
mod tests {
    use super::*;

    struct MockMessenger {
        sent_messages: Vec<String>,
    }

    impl MockMessenger {
        fn new() -> MockMessenger {
            MockMessenger {
                sent_messages: vec![],
            }
        }
    }

    impl Messenger for MockMessenger {
        fn send(&self, message: &str) {
	        // this line doesn't compile, since self is immutable!
            self.sent_messages.push(String::from(message));
        }
    }

    #[test]
    fn it_sends_an_over_75_percent_warning_message() {
        let mock_messenger = MockMessenger::new();
        let mut limit_tracker = LimitTracker::new(&mock_messenger, 100);

        limit_tracker.set_value(80);

        assert_eq!(mock_messenger.sent_messages.len(), 1);
    }
}
```

The problem here is the trait we're trying to implement has a `send` function that takes in an immutable reference to the messenger, but we want our mock messenger to mutate when it is called.

Here's how to fix it:

```rust
#[cfg(test)]
mod tests {
    use super::*;
    use std::cell::RefCell;

    struct MockMessenger {
        sent_messages: RefCell<Vec<String>>,
    }

    impl MockMessenger {
        fn new() -> MockMessenger {
            MockMessenger {
                sent_messages: RefCell::new(vec![]),
            }
        }
    }

    impl Messenger for MockMessenger {
        fn send(&self, message: &str) {
            self.sent_messages.borrow_mut().push(String::from(message));
        }
    }

    #[test]
    fn it_sends_an_over_75_percent_warning_message() {
        // --snip--

        assert_eq!(mock_messenger.sent_messages.borrow().len(), 1);
    }
}
```
Now we just wrapped the vector in a `RefCell`, which means we can `borrow_mut()` to get a mutable reference (even out of an immutable cell!).

## How `RefCell<T>` Tracks Borrows

A `RefCell<T>` exposes two methods, a `borrow` and `borrow_mut`, which return `Ref<T>` and `RefMut<T>`, respectively.

The `RefCell<T>`, at runtime, keeps track of how many `Ref<T>` and `RefMut<T>` it has given out (when they are dropped, the counters also go down). Then, if there are ever:

* Both an immutable and mutable reference, or
* More than one mutable reference

Out at the same time, then the code will panic. Checking at runtime means that you might catch bugs later on in development. It also incurs a slight runtime performance penalty.

## Multiple Owners of Mutable Data

By using a `Rc<Refcell<T>>`, you can have multiple owners of mutable data!

---

**Next:** [[Reference Cycles]]

