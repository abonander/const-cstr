# `const-cstr` [![](https://img.shields.io/crates/v/const-cstr.svg)](crates.io/crates/const-cstr)
Create static C-compatible strings from Rust string literals.

Usage
------
Cargo.toml:
```toml
[dependencies]
const-cstr = "0.1"
```

Crate root:
```rust
#[macro_use] extern crate const_cstr;
```

Example
-------
```rust
 #[macro_use] extern crate const_cstr;
 // Just for the `libc::c_char` type alias.
 extern crate libc;
     
 use std::ffi::CStr;

 const_cstr! {
     HELLO_CSTR = "Hello, world!";

     // Multiple declarations can be made with one invocation.
     // GOODNIGHT_CSTR = "Goodnight, sun!";

     // But only with the same visibility:
     // pub GOODNIGHT_CSTR = "Goodnight, sun!";
     // ^~~ Error: expected identifier, found `pub` 
 }

 // Imagine this is an `extern "C"` function linked from some other lib.
 unsafe fn print_c_string(cstr: *const libc::c_char) {
     println!("{}", CStr::from_ptr(cstr).to_str().unwrap());
 }

 fn main() {
     // When just passed a literal, returns an rvalue instead.
     let goodnight_cstr = const_cstr!("Goodnight, sun!");

     unsafe {
         print_c_string(HELLO_CSTR.as_ptr());
         print_c_string(goodnight_cstr.as_ptr());
     }
 }
 ```

 Prints:

 ```notest
 Hello, world!
 Goodnight, sun!
 ```

License
-------
MIT

