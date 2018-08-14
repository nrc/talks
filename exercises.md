# Exercises

TODO each exercise should have a bonus question and an easy question

## Exercise 1 (understanding ownership)

* Why don't Box (or other owning types) need a lifetime parameter?
* Why is it an ownership graph and not a tree?
* What is at the roots of the ownership graph?
* How does ownership relate to mutability?

## Exercise 2 (types and control flow)

Write the following functions as succinctly as you can (you'll want to check the
[std docs](https://doc.rust-lang.org/std/index.html)):

TODO compiling version

```rust
fn foo(input: Option<i32>) -> Option<i32> {
    if input.is_none() {
        return None;
    }

    let input = input.unwrap();
    if input < 0 {
        return None;
    }
    Some(input)
}

fn bar(input: Option<i32>) -> Result<i32, ErrNegative> {
    match foo(input) {
        Some(n) => Ok(n),
        None => Err(ErrNegative),
    }
}

// These are just to make the program compile.
struct ErrNegative;
fn main() {}
```

## Exercise 3 (iteration)

Lower the following program to a simpler one without `while let` by using `loop`,
`match`, and `break`.

```rust
let vec = vec![0, 1, 2, 3];

let mut iter = (&vec).into_iter();
while let Some(v) = iter.next() {
    println!(“{}”, v);
}
```


## Exercise 4 (error handling)

Convert the first three functions to an error handling strategy, rather than
a panicking strategy.

TODO check compiles

```rust
// Remove the `expect`s from these two functions
fn read_config() -> ConfigFile {
    let file = read_file().expect("could not open file");

    write_to_log().expect("could not write to log file");
    file
}

impl Server {
    fn startup(self) -> ListeningServer {
        let config = read_config();
        let port = open_port().expect("could not open port");
        self.configure(config, port)
    }
}

fn main() {
    let server = Server::new();
    let server = server.startup();
    while let Some(packet) = server.listen() {
        // ...
    }
}

// Helper functions and types, don't change these
fn write_to_log() -> Result<(), DiskError> { Ok(()) }
fn open_port() -> Result<Port, NetworkError> { Port }
fn read_file() -> Result<ConfigFile, DiskError> { Ok(ConfigFile) }

struct Server;
struct Port;
struct DiskError;
struct NetworkError;
struct ConfigFile;
struct ListeningServer;

impl Server {
    fn new() -> Server { Server }
    fn configure(self) -> ListeningServer { ListeningServer }
}
```

## Exercise 5 (design using ownership)

Rewrite the following in Rust, paying attention to ownership in the design.

TODO extra instructions

```c
struct SlabAllocator {
    unsigned char* data_start;
    int data_len;
    unsigned char* next;
}

*unsigned char allocate(SlabAllocator* slab, int bytes) { /* ... */ }

void deallocate(SlabAllocator* slab,
                unsigned char* data,
                int len)
{ /* ... */ }

void destroy(SlabAllocator* slab) { /* ... */ }

int main(int argc, char* argv[]) {
    SlabAllocator alloc = { /* ... */ };
    *unsigned char buf = allocate(&alloc, 64);
    // ...
    deallocate(&alloc, buf, 64);
    destroy(&alloc);
    return 0;
}
```
