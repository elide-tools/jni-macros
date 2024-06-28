# java_native

[![crates.io](https://img.shields.io/crates/v/java_native.svg)](https://crates.io/crates/java_native)
[![Docs](https://docs.rs/java_native/badge.svg)](https://docs.rs/java-native/)
[![License](https://img.shields.io/badge/license-MIT-blue.svg)]()

`java_native` is a JNI-compatible method signature generator for Rust libraries; it builds upon the
[`jni_fn`](https://gitlab.com/antonok/jni_fn) by `@antonok` on Gitlab.

This crate was designed for use with the [`jni`](https://crates.io/crates/jni) crate, which exposes JNI-compatible type bindings.
Although it's possible to use `jni` without `java_native`, the procedural macros defined here will make it easier to write the method signatures correctly.

## How to use

Check the [`jni`](https://github.com/jni-rs/jni-rs) repo to get started with your first Rust JNI bindings.

Note the function signatures in the [`jni` example project](https://github.com/jni-rs/jni-rs/blob/master/example/mylib/src/lib.rs), which must be transcribed 100% correctly to avoid runtime panics in your JVM project:

```rust
#[no_mangle]
pub extern "system" fn Java_HelloWorld_hello(
    // ...
```

Instead, `java_native` can automatically generate the correct function signature based on the package name (`HelloWorld`) and function name (`hello`):

```rust
use jni_fn::jni_fn;

#[jni_fn("HelloWorld")]
pub fn hello(
    // ...
```

The `jni` macro is especially useful in more complicated examples - you don't want to figure [this](https://github.com/signalapp/libsignal-client/blob/2651de993ac29e40bfe2980d4d9c43198e1e6cd8/rust/bridge/jni/src/lib.rs#L19-L20) out manually! With `jni`, all you need is:

```rust
#[jni("org.signal.client.internal.Native")]
pub unsafe fn IdentityKeyPair_Deserialize(
    // ...
```

## Exporting JNI hooks

For hook functions like `JNI_OnLoad` or `JNI_OnLoad_libname`, use:

```rust
// as a dynamic library:
#[on_load]  // becomes `JNI_OnLoad`
pub unsafe fn on_load(vm: JavaVM) -> jint {
    // your init code...
    JNI_VERSION_1_8
}

// as a static library:
#[on_load(example)]  // becomes `JNI_OnLoad_example`
pub unsafe fn on_load(vm: JavaVM) -> jint {
    // your init code...
    JNI_VERSION_1_8
}
```

There is an `on_load` and `on_unload` attribute; pass an attribute name for a static binding.

Visit the [docs](https://docs.rs/jni-fn/) for more instructions and examples.
