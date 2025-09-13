In Rust, lifetime annotations are a mechanism used to ensure the validity of references at compile time, preventing common issues like dangling pointers. They are a form of generic parameter that describes how long a reference is valid relative to other references or the scope of the data it points to.
Here's how lifetime annotations are used to ensure reference validity: Declaring Lifetime Parameters.
Lifetime parameters are declared within angle brackets, similar to generic type parameters, but they start with an apostrophe (e.g., 'a, 'b).
```rust
    struct MyStruct<'a> {
        data: &'a str,
    }
```
Annotating References.

When a struct or function takes or returns references, these references can be annotated with the declared lifetime parameters. 
This tells the Rust compiler (specifically, the borrow checker) how the lifetimes of these references relate to each other.

```rust
    fn longest<'a>(x: &'a str, y: &'a str) -> &'a str {
        if x.len() > y.len() {
            x
        } else {
            y
        }
    }
```
In this longest function, the 'a annotation indicates that both input references x and y, as well as the returned reference, must have the same lifetime. This ensures that the returned reference is valid for at least as long as the shortest-lived input reference. Ensuring Data Outlives References.
The core principle of lifetimes is that a reference cannot outlive the data it points to. The borrow checker uses lifetime annotations to enforce this rule. If you have a struct holding a reference, the lifetime annotation on the struct ensures that an instance of that struct cannot exist longer than the data it references.

```rust
    struct ImportantExcerpt<'a> {
        part: &'a str,
    }

    fn main() {
        let novel = String::from("Call me Ishmael. Some years ago...");
        let first_sentence = novel.split('.').next().expect("Could not find a '.'");
        let i = ImportantExcerpt { part: first_sentence };
        // 'i' cannot outlive 'novel', because 'i.part' references data owned by 'novel'.
    }
```
In essence, lifetime annotations are descriptive, not prescriptive. They don't change how long a reference actually lives, but rather provide the compiler with the necessary information to verify that the relationships between references are sound and prevent dangling references or other memory safety issues at compile time.
