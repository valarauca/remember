function casting
---

Basically let us say we're doing something like

```rust
/// This seems reasonable?
pub type SOMETHING = &'static fn(i32,i32) -> i32;

pub fn foo(x: i32, y: i32) -> i32 { x + y }
pub fn bar(x: i32, y: i32) -> i32 { x - y }

/// This makes sense as well right?
pub const ARRAY_OF_SOMETHING: &'static [SOMETHING] = &[
    &foo,
    &bar,
];
```

You actually want to do:

```rust
pub type SOMETHING = &'static (dyn Fn(i32,i32) -> i32 + 'static);

pub fn foo(x: i32, y: i32) -> i32 { x + y }
pub fn bar(x: i32, y: i32) -> i32 { x - y }

/// This makes sense as well right?
pub const ARRAY_OF_SOMETHING: &'static [SOMETHING] = &[
    &foo,
    &bar,
];

