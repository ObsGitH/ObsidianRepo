A very short resume of what I learned


# 1
# 2
# 3

# 4

# 5

# 6
# 7
# 8

# 9
# 10

## Generic Types

## Traits

 Equivalent to a interface but with some differences 

  We can use _trait bounds_ to specify that a generic type can be any type that has certain behavior. 

 the methods that implement a trait need to define behavior for all of the methods in that trait that have only been declared and do not posses default behavior.

 methods in a trait that have default behavior can be overriden when implementing the trait on a data type.

 when it comes to scope, you can implement internal traits on external types and external traits on internal types. But thanks to **coherence**, more specifically the **external rule** external traits cannot be implemented on external types. (internal or external in relationship to crates)

 In a trait, methods with default behavior can call methods with undefined behavior since a type that implements said trait must defined behavior for methods that dont have any. 

 We pass traits as parameters to functions like this: `fn notify(item: &impl Summary) {}`. This parameter accepts any type that implements the specified trait.  we can call any methods on `item` that come from the `Summary` trait. This is syntax sugar for **Trait Bound Syntax** an is apropiate when we want this function to allow `parameter1` and `parameter2` to have different types (as long as both types implement `Summary`).

 **Trait Bound Syntax** : `fn notify<T: Summary>(item: &T)` this is apropiate when the `parameter1` and `parameter2` parameters constrains the function such that the concrete type of the value passed as an argument for `parameter1` and `parameter2` must be the same.

Specifying Multiple Trait Bounds: with sintatic sugar = `pub fn notify(item: &(impl Summary + Display)) {`, trait bound: `pub fn notify<T: Summary + Display>(item: &T) {`

 Trait Bounds with where clauses: `pub fn notify<T: Summary + Display>(item: &T) {beginning of the body`. To improve readability:                                                                        `fn some_function<T, U>(t: &T, u: &U) -> i32                                        where                                                                              T:                                                                           Display + Clone,                                                                  U: Clone + Debug,                                                                  {`

Return a trait with `fn returns_summarizable() -> impl Summary {`. This means that the function will return a concrete type that implements the abstract trait `Summary`.  Especially useful in the context of closures and iterators, which we cover in Chapter 13, because it guarantees that the type that is returned can be iterated over. You can only use `impl Trait` if you’re returning a single concrete type, you cannot have a function that will return concrete type 1 sometimes and concrete type 2 other times, even if they implement `summary`! This is due to how `impl Summary` syntax is implemented in the compiler, but we are goin to lear how to do it in Chapter 17

 Trait Bounds to Conditionally Implement Methods: By using a trait bound with an `impl` block that uses generic type parameters, we can implement methods conditionally for types that implement the specified traits -> `struct Pair<T> {body of struct`,                                 `impl<T: Display + PartialOrd> Pair<T> { fn cmp_display(&self) {` ->                               In the `impl` block, `Pair<T>` only implements the `cmp_display` method if its inner type `T` implements the `PartialOrd` trait that enables comparison _and_ the `Display` trait that enables printing. Implementations of a trait on any type that satisfies the trait bounds are called _blanket implementations_ and are extensively used in the Rust standard library. Blanket implementations appear in the documentation for the trait in the “Implementors” section
 
## Validating references with Lifetimes - New Concept!

No way to resume, you need to read it.

### [Lifetime Annotations in Struct Definitions](https://doc.rust-lang.org/book/ch10-03-lifetime-syntax.html#lifetime-annotations-in-struct-definitions)

# 11 Write Automated Tests

## How To Write Tests

test functions typically perform these three actions:

1. Set up any needed data or state.
2. Run the code you want to test.
3. Assert the results are what you expect.
4. 
features Rust provides specifically for writing tests that take these actions  include the `test` attribute, a few macros, and the `should_panic` attribute.

if the program panicks during a test the test failed.

macros:
- `panic`! makes the test fail
- `assert!` panics  if the expression passed to it is false. Very good for testing anything that returns a boolean value: expressions, methods, etc. Obviously if you want the expressions/method/whatever pass the test if it return false, then you need to negate it.
		- `assert_ne!`, `assert_eq!`. test for inequality/equality between the result of the code under test and the value you expect the code to return. the order in which we specify the value we expect and the value the code produces doesn’t matter. The `assert_ne!` macro will pass if the two values we give it are not equal and fail if they’re equal. This macro is most useful for cases when we’re not sure what a value _will_ be, but we know what the value definitely _shouldn’t_ be. When the assertions fail, these macros print their arguments using debug formatting, which means the values being compared must implement the `PartialEq` and `Debug` traits. his is usually as straightforward as adding the `#[derive(PartialEq, Debug)]` annotation to your struct or enum definition.  Any arguments specified after the required arguments are passed along to the `format!` macro so you can pass a format string that contains `{}` placeholders and values to go in those placeholders

attributes:
- `#[test]`: tells the compiler that this function is for testing
- `#[should_panic]` - Inverts the point of the usual test. Now this test will try to make the code that is testing panic and will fail if it cannot make the code panic. Some code is just tested more efficiently if we can determine that when the code needs to fail it will fail.
	- `should_panic(expected = "the panick message to you expect")`: A `should_panic` test would pass even if the test panics for a different reason from the one we were expecting. To make `should_panic` tests more precise, we can add an optional `expected` parameter to the `should_panic` attribute.
- `#[ignore]`: ignores the test when executing `cargo test` command. Very useful for when some tests are particularly heavy, so its better to execute them individually rather than as a whole. When you’re at a point where it makes sense to check the results of the `ignored` tests and you have time to wait for the results, you can run `cargo test -- --ignored` instead. If you want to run all tests whether they’re ignored or not, you can run `cargo test -- --include-ignored`.

Test Return Types:
- riting tests so they return a `Result<T, E>` enables you to use the question mark operator in the body of tests, which can be a convenient way to write tests that should fail if any operation within them returns an `Err` variant.

You can’t use the `#[should_panic]` annotation on tests that use `Result<T, E>`. To assert that an operation returns an `Err` variant, _don’t_ use the question mark operator on the `Result<T, E>` value. Instead, use `assert!(value.is_err())`.

When you run your tests with the `cargo test` command, Rust builds a test runner binary that runs the annotated functions and reports on whether each test function passes or fails.

Whenever we make a new library project with Cargo, a test module with a test function in it is automatically generated for us. This module gives you a template for writing your tests so you don’t have to look up the exact structure and syntax every time you start a new project. You can add as many additional test functions and as many test modules as you want!



## Controlling How Tests Run

The default behavior of the binary produced by `cargo test` is to run all the tests in parallel and capture output generated during test runs, preventing the output from being displayed and making it easier to read the output related to the test results. You can, however, specify command line options to change this default behavior.


`$ cargo test -- --test-threads=1` to run synchronously instead of parallel and control the amount of threads that are going to be used by cargo

`$ cargo test -- --show-output` to show output of methods/macros/whatever that generate stdout or stderr data.

`$ cargo test add`.  runs every test that has "add" in its function signature's name. Filtering the tests. 


# 12

# 13

# 14

# 15

# 16

# 17 

# 18 

# 19 

# 20