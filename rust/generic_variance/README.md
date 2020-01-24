generic variance in rust
---

Generic Variance in Rust is a quick and easy method to
generalize interfaces to accept multiple types. It is
a powerful tool.

# Covariance, Contravariance, Invariance.

What does this mean?


See Diagram:

![Numbers](/rust/variance/Number-systems.svg.png)

* ℝ: Real Numbers (any number; negative & positive, fractions, infinite decimals, pi, etc.)
* ℚ: Rational Numbers (fractions; negative & positive)
* ℤ: Integers (whole numbers; negative & positive)
* ℕ: Numbers (non-negative whole numbers: 0, 1, 2, 3, 4, ....)

## Covariance

You can say ℕ is Covariant of ℤ (for some transformation, operation, function, etc.)
Because ℕ is a sub-set (or more specific set) of ℤ. So where ever you need a ℤ, you can pass a ℕ.

## Contravariant

ℕ is contravariant to ℚ, because all ℚ's are not ℕ's, only some of them.
So where ever you need an ℕ, you cannot pass a ℚ. **BUT** where ever you
need a ℚ you can pass an ℕ, because is co-variance.


citations:

* [wikipedia page](https://en.wikipedia.org/wiki/Covariance_and_contravariance_%28computer_science%29)

# Actual Rust

Covariant typing is extremely useful for transformations with more than 1 type for example:

```rust
pub enum MyEnum {
    Num(i64),
    String(String),
    Float(f64),
}
impl From<i64> for MyEnum {
    fn from(x: i64) -> Self {
        MyEnum::Num(x)
    }
}
impl From<String> for MyEnum {
    fn from(x: String) -> Self {
        MyEnum::String(x)
    }
}
impl From<f64> for MyEnum {
    fn from(x: f64) -> Self {
        MyEnum::Float(x)
    }
}
```

Can let us define another type like:

```rust
pub struct MyEnumCollection {
    data: Vec<MyEnum>
}
impl MyEnumCollection {
    pub fn push<T>(&mut self, arg: T)
    where MyEnum: From<T>
    {
        self.data.push(MyEnum::from(arg));
    }
}
```

The `From<T>` trait has created a **covariant** relationship between types (kind of).

# Other useful traits

This trick can be used anywhere where a trait is encoding additional typing information.

Namely `Deref`, `Iterator`, `AsRef`, `Index`, etc.

