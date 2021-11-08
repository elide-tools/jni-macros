# jni_fn

[![crates.io](https://img.shields.io/crates/v/jni_fn.svg)](https://crates.io/crates/jni_fn)
[![Docs](https://docs.rs/jni_fn/badge.svg)](https://docs.rs/jni-fn/)
[![License](https://img.shields.io/badge/license-MIT-blue.svg)]()

`jni_fn` is a JNI-compatible method signature generator for Rust libraries.

This crate was designed for use with the [`jni`](https://crates.io/crates/jni) crate, which exposes JNI-compatible type bindings.
Although it's possible to use `jni` without `jni_fn`, the procedural macro defined here will make it easier to write the method signatures correctly.

## How to use

Check the [`jni`](https://github.com/jni-rs/jni-rs) repo to get started with your first Rust JNI bindings.

Note the function signatures in the [`jni` example project](https://github.com/jni-rs/jni-rs/blob/master/example/mylib/src/lib.rs), which must be transcribed 100% correctly to avoid runtime panics in your JVM project:

```rust
#[no_mangle]
pub extern "system" fn Java_HelloWorld_hello(
    // ...
```

Instead, `jni_fn` can automatically generate the correct function signature based on the package name (`HelloWorld`) and function name (`hello`):

```rust
use jni_fn::jni_fn;

#[jni_fn("HelloWorld")]
pub fn hello(
    // ...
```

`jni_fn` is especially useful in more complicated examples - you don't want to figure [this](https://github.com/signalapp/libsignal-client/blob/2651de993ac29e40bfe2980d4d9c43198e1e6cd8/rust/bridge/jni/src/lib.rs#L19-L20) out manually! With `jni_fn`, all you need is:

```rust
#[jni_fn("org.signal.client.internal.Native")]
pub unsafe fn IdentityKeyPair_Deserialize(
    // ...
```

Visit the [docs](https://docs.rs/jni-fn/) for more instructions and examples.
