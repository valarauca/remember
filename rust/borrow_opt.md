# Borrow

Follows is a simple function that makes pattern matching on function arguments very simple. It exploits the `std::borrow::Borrow` interface.


```rust
pub fn borrow_opt<'a,A,B>(arg: &'a Option<A>) -> Option<&'a B>
where
  A: std::borrow::Borrow<B>,
  B: 'a + ?Sized,
{
    arg.as_ref()
      .map(<A as std::borrow::Borrow<B>>::borrow)
}
```

This function is a little more useful than the standard libraries [`as_ref`](https://doc.rust-lang.org/std/option/enum.Option.html#method.as_ref) which is it comparable too.

The notable difference is this will convert (for example):

* `&Option<String>` -> `Option<&str>`
* `&Option<Vec<T>>` -> `Option<&[u8]>`

This function is primarily useful when a structure has optional fields, but you want to present a nice immutable API to the user.
