# cargo-equip

[![CI](https://github.com/qryxip/cargo-equip/workflows/CI/badge.svg)](https://github.com/qryxip/cargo-equip/actions?workflow=CI)
[![codecov](https://codecov.io/gh/qryxip/cargo-equip/branch/master/graph/badge.svg)](https://codecov.io/gh/qryxip/cargo-equip/branch/master)
[![dependency status](https://deps.rs/repo/github/qryxip/cargo-equip/status.svg)](https://deps.rs/repo/github/qryxip/cargo-equip)
[![Crates.io](https://img.shields.io/crates/v/cargo-equip.svg)](https://crates.io/crates/cargo-equip)
[![Crates.io](https://img.shields.io/crates/l/cargo-equip.svg)](https://crates.io/crates/cargo-equip)

[日本語](https://github.com/qryxip/cargo-equip/blob/master/README-ja.md)

A Cargo subcommand to bundle your code into one `.rs` file for competitive programming.

## Example

[Sqrt Mod - Library-Cheker](https://judge.yosupo.jp/problem/sqrt_mod)

```toml
[package]
name = "solve"
version = "0.0.0"
edition = "2018"

[dependencies]
ac-library-rs-parted             = { git = "https://github.com/qryxip/ac-library-rs-parted" }
ac-library-rs-parted-convolution = { git = "https://github.com/qryxip/ac-library-rs-parted" }
ac-library-rs-parted-dsu         = { git = "https://github.com/qryxip/ac-library-rs-parted" }
ac-library-rs-parted-fenwicktree = { git = "https://github.com/qryxip/ac-library-rs-parted" }
ac-library-rs-parted-lazysegtree = { git = "https://github.com/qryxip/ac-library-rs-parted" }
ac-library-rs-parted-math        = { git = "https://github.com/qryxip/ac-library-rs-parted" }
ac-library-rs-parted-maxflow     = { git = "https://github.com/qryxip/ac-library-rs-parted" }
ac-library-rs-parted-mincostflow = { git = "https://github.com/qryxip/ac-library-rs-parted" }
ac-library-rs-parted-modint      = { git = "https://github.com/qryxip/ac-library-rs-parted" }
ac-library-rs-parted-scc         = { git = "https://github.com/qryxip/ac-library-rs-parted" }
ac-library-rs-parted-segtree     = { git = "https://github.com/qryxip/ac-library-rs-parted" }
ac-library-rs-parted-string      = { git = "https://github.com/qryxip/ac-library-rs-parted" }
ac-library-rs-parted-twosat      = { git = "https://github.com/qryxip/ac-library-rs-parted" }
input                            = { path = "/path/to/input"                                }
output                           = { path = "/path/to/output"                               }
tonelli_shanks                   = { path = "/path/to/tonelli_shanks"                       }
# ...
```

```rust
// Uncomment this line if you don't use your libraries. (`--check` still works)
//#![cfg_attr(cargo_equip, cargo_equip::skip)]

#[macro_use]
extern crate input as _;

use acl_modint::ModInt;
use std::io::Write as _;
use tonelli_shanks::ModIntBaseExt as _;

fn main() {
    input! {
        yps: [(u32, u32)],
    }

    output::buf_print(|out| {
        macro_rules! println(($($tt:tt)*) => (writeln!(out, $($tt)*).unwrap()));
        for (y, p) in yps {
            ModInt::set_modulus(p);
            if let Some(sqrt) = ModInt::new(y).sqrt() {
                println!("{}", sqrt);
            } else {
                println!("-1");
            }
        }
    });
}
```

↓

```console
❯ cargo equip --resolve-cfgs --remove docs --minify libs --rustfmt --check -o ./bundled.rs
     Running `/home/ryo/.rustup/toolchains/stable-x86_64-unknown-linux-gnu/bin/cargo check --message-format json -p -p 'ac-library-rs-parted-build:0.1.0' -p 'ac-library-rs-parted-convolution:0.1.0' -p 'ac-library-rs-parted-dsu:0.1.0' -p 'ac-library-rs-parted-fenwicktree:0.1.0' -p 'ac-library-rs-parted-internal-bit:0.1.0' -p 'ac-library-rs-parted-internal-math:0.1.0' -p 'ac-library-rs-parted-internal-queue:0.1.0' -p 'ac-library-rs-parted-internal-scc:0.1.0' -p 'ac-library-rs-parted-internal-type-traits:0.1.0' -p 'ac-library-rs-parted-lazysegtree:0.1.0' -p 'ac-library-rs-parted-math:0.1.0' -p 'ac-library-rs-parted-maxflow:0.1.0' -p 'ac-library-rs-parted-mincostflow:0.1.0' -p 'ac-library-rs-parted-modint:0.1.0' -p 'ac-library-rs-parted-scc:0.1.0' -p 'ac-library-rs-parted-segtree:0.1.0' -p 'ac-library-rs-parted-string:0.1.0' -p 'ac-library-rs-parted-twosat:0.1.0' -p 'anyhow:1.0.34' -p 'byteorder:1.3.4' -p 'num-traits:0.2.14' -p 'proc-macro2:1.0.10' -p 'ryu:1.0.5' -p 'serde:1.0.113' -p 'serde_derive:1.0.113' -p 'serde_json:1.0.59' -p 'syn:1.0.17' -p 'typenum:1.12.0'`
    Finished dev [unoptimized + debuginfo] target(s) in 0.03s
     Running `/home/ryo/.cargo/bin/rustup run nightly cargo udeps --output json -p solve --bin solve`
    Checking solve v0.0.0 (/home/ryo/src/local/a/solve)
    Finished dev [unoptimized + debuginfo] target(s) in 0.16s
info: Loading save analysis from "/home/ryo/src/local/a/solve/target/debug/deps/save-analysis/solve-2970d6e10b9c0877.json"
    Bundling the code
    Checking cargo-equip-check-output-nq4nm7zkj9vtgbd9 v0.1.0 (/tmp/cargo-equip-check-output-nq4nm7zkj9vtgbd9)
    Finished dev [unoptimized + debuginfo] target(s) in 0.35s
```

[Submit Info #29083 - Library-Checker](https://judge.yosupo.jp/submission/29083)

## Installation

Install a `nightly` toolchain and [cargo-udeps](https://github.com/est31/cargo-udeps) first.

```console
❯ rustup update nightly
```

```console
❯ cargo install --git https://github.com/est31/cargo-udeps # for est31/cargo-udeps#80
```

### Crates.io

```console
❯ cargo install cargo-equip
```

### `master`

```console
❯ cargo install --git https://github.com/qryxip/cargo-equip
```

### GitHub Releases

[Releases](https://github.com/qryxip/cargo-equip/releases)

## Usage

Follow these constrants when you writing libraries to bundle.

1. Do not put items with the name names of `#[macro_export]`ed macros in each crate root.

    cargo-equip inserts `pub use crate::{ these_names };` in `mod lib_name`.
    Use `#[macro_use]` to import macros in `bin`.

    ```rust
    // in main source code

    #[macro_use]
    extern crate input as _;
    ```

    `extern crate` items in `bin`s are commented-out.

    ```rust
    // in main source code

    /*#[macro_use]
    extern crate input as _;*/ // `use crate::$name;` is inserted if the rename is not `_`
    ```

2. Do not resolve names of crates to bundle directly from [extern prelude](https://doc.rust-lang.org/reference/items/extern-crates.html#extern-prelude).

    Mount them somewhere with `extern crate` and refer them with relative paths.

    cargo-equip replaces `extern crate` items with `use crate::extern_crate_name_in_main_crate;` except for crates available on AtCoder or CodinGame. (e.g. `itertools`)
    [Rename](https://doc.rust-lang.org/cargo/reference/specifying-dependencies.html#renaming-dependencies-in-cargotoml) the libraries not to use directly.

    ```rust
    extern crate __another_lib as another_lib;

    use self::another_lib::foo::Foo; // Prepend `self::` to make compatible with Rust 2015
    ```

3. Use `$crate` instead of `crate` in macros.

    cargo-equip replaces `$crate` in `macro_rules!` with `$crate::extern_crate_name_in_main_crate`.
    `crate`s are not modified.

4. Do not use absolute path as possible.

    cargo-equip replaces `crate` with `crate::extern_crate_name_in_main_crate` and `pub(crate)` with `pub(in crate::extern_crate_name_in_main_crate)`.

    However I cannot ensure this works well.
    Use `self::` and `super::` instead of `crate::`.

    ```diff
    -use crate::foo::Foo;
    +use super::foo::Foo;
    ```

5. Split into small separate crates as possible.

    cargo-equip does not search "dependencies among items".

    On a website other except AtCoder, Split your library into small crates to fit in 64KiB.

    ```console
    .
    ├── input
    │   ├── Cargo.toml
    │   └── src
    │       └── lib.rs
    ├── output
    │   ├── Cargo.toml
    │   └── src
    │       └── lib.rs
    ⋮
    ```

When you finish preparing your libraries, add them to `[dependencies]` of the `bin`.
If you generate packages automatically with a tool, add to its template.

If you want to use [rust-lang-ja/ac-library-rs](https://github.com/rust-lang-ja/ac-library-rs), use [qryxip/ac-library-rs-parted](https://github.com/qryxip/ac-library-rs-parted) instead.
ac-library-rs-parted is a collection of 17 crates that process the real ac-library-rs in a `custom-build`.
The `custom-build` is written with `syn 1.0.17` and `proc-macro2 1.0.10` in order not to break [lockfiles for AtCoder](https://github.com/qryxip/cargo-compete/blob/ba8e0e747ed90768d9f50f3061374162dade8450/resources/atcoder-cargo-lock.toml).

```toml
[dependencies]
ac-library-rs-parted             = { git = "https://github.com/qryxip/ac-library-rs-parted" }
ac-library-rs-parted-convolution = { git = "https://github.com/qryxip/ac-library-rs-parted" }
ac-library-rs-parted-dsu         = { git = "https://github.com/qryxip/ac-library-rs-parted" }
ac-library-rs-parted-fenwicktree = { git = "https://github.com/qryxip/ac-library-rs-parted" }
ac-library-rs-parted-lazysegtree = { git = "https://github.com/qryxip/ac-library-rs-parted" }
ac-library-rs-parted-math        = { git = "https://github.com/qryxip/ac-library-rs-parted" }
ac-library-rs-parted-maxflow     = { git = "https://github.com/qryxip/ac-library-rs-parted" }
ac-library-rs-parted-mincostflow = { git = "https://github.com/qryxip/ac-library-rs-parted" }
ac-library-rs-parted-modint      = { git = "https://github.com/qryxip/ac-library-rs-parted" }
ac-library-rs-parted-scc         = { git = "https://github.com/qryxip/ac-library-rs-parted" }
ac-library-rs-parted-segtree     = { git = "https://github.com/qryxip/ac-library-rs-parted" }
ac-library-rs-parted-string      = { git = "https://github.com/qryxip/ac-library-rs-parted" }
ac-library-rs-parted-twosat      = { git = "https://github.com/qryxip/ac-library-rs-parted" }
```

The constraints for `bin`s are:

1. Do not import macros with `use`. Use them with `#[macro_use]` or with qualified paths.
2. If you create `mod`s, do not resolve names of crates to bundle directly from [extern prelude](https://doc.rust-lang.org/reference/items/extern-crates.html#extern-prelude).

```rust
// Uncomment this line if you don't use your libraries. (`--check` still works)
//#![cfg_attr(cargo_equip, cargo_equip::skip)]

#[macro_use]
extern crate input as _;

use std::io::Write as _;

fn main() {
    input! {
        n: usize,
    }

    output::buf_print(|out| {
        macro_rules! println(($($tt:tt)*) => (writeln!(out, $($tt)*).unwrap()));
        for i in 1..=n {
            match i % 15 {
                0 => println!("Fizz Buzz"),
                3 | 6 | 9 | 12 => println!("Fizz"),
                5 | 10 => println!("Buzz"),
                _ => println!("{}", i),
            }
        }
    });
}
```

Then execute `cargo-equip`.

```console
❯ cargo equip --bin "$name"
```

cargo-equip outputs code like this.
It gives tentative `extern_crate_name`s like `__package_name_0_1_0` to dependencies of the dependencies.

```diff
+//! # Bundled libraries
+//!
+//! ## `input` (private)
+//!
+//! ### `extern_crate_name`
+//!
+//! `input`
+//!
+//! ## `output` (private)
+//!
+//! ### `extern_crate_name`
+//!
+//! `output`

// Uncomment this line if you don't use your libraries. (`--check` still works)
//#![cfg_attr(cargo_equip, cargo_equip::skip)]

-#[macro_use]
-extern crate input as _;
+/*#[macro_use]
+extern crate input as _;*/

 use std::io::Write as _;

 fn main() {
     input! {
         n: usize,
     }

     output::buf_print(|out| {
         macro_rules! println(($($tt:tt)*) => (writeln!(out, $($tt)*).unwrap()));
         for i in 1..=n {
             match i % 15 {
                 0 => println!("Fizz Buzz"),
                 3 | 6 | 9 | 12 => println!("Fizz"),
                 5 | 10 => println!("Buzz"),
                 _ => println!("{}", i),
             }
         }
     });
 }
+
+// The following code was expanded by `cargo-equip`.
+
+#[allow(dead_code)]
+mod input {
+    // ...
+}
+
+#[allow(dead_code)]
+mod output {
+    // ...
+}
```

cargo-equip does the following modification.

- `bin`
    - If a `#![cfg_attr(cargo_equip, cargo_equip::skip)]` was found, skips the remaining processes, does `--check`, and outputs the code as-is.
    - If any, expands `mod $name;` recursively indenting them except those containing multi-line literals.
    - Processes `extern crate` items.
    - Prepends a doc comment.
    - Appends the expanded libraries.
- `lib`s
    - Expands `mod $name;` recursively.
    - Processes `crate` paths.
    - Processes `extern crate` items.
    - Processes `macro_rules!`.
    - Processes `#[cfg(..)]` attributes when `--resolve-cfg` is specified.
    - Removes doc comments when `--remove docs` is specified.
    - Removes comments when `--remove comments` is specified.
- Whole
    - Minifies the whole output when `--minify all` is specified.
    - Formats the output when `--rustfmt` is specified.

## Options

### `--resolve-cfgs`

1. Removes `#[cfg(always_true_predicate)]` (e.g. `cfg(feature = "enabled-feature")`).
2. Removes items with `#[cfg(always_false_preducate)]` (e.g. `cfg(test)`, `cfg(feature = "disable-feature")`).

Predicates are evaluated according to:

- [`test`](https://doc.rust-lang.org/reference/conditional-compilation.html#test): `false`
- [`proc_macro`](https://doc.rust-lang.org/reference/conditional-compilation.html#proc_macro): `false`
- `cargo_equip`: `true`
- [`feature`](https://doc.rust-lang.org/cargo/reference/features.html): `true` for those present
- Otherwise: unknown

```rust
#[allow(dead_code)]
pub mod a {
    pub struct A;

    #[cfg(test)]
    mod tests {
        #[test]
        fn it_works() {
            assert_eq!(2 + 2, 4);
        }
    }
}
```

↓

```rust
#[allow(dead_code)]
pub mod a {
    pub struct A;
}
```

### `--remove <REMOVE>...`

Removes

- doc comments (`//! ..`, `/// ..`, `/** .. */`, `#[doc = ".."]`) with `--remove docs`.
- comments (`// ..`, `/* .. */`) with `--remove comments`.

```rust
#[allow(dead_code)]
pub mod a {
    //! A.

    /// A.
    pub struct A; // aaaaa
}
```

↓

```rust
#[allow(dead_code)]
pub mod a {
    pub struct A;
}
```

### `--minify <MINIFY>`

Minifies

- each expaned library with `--minify lib`.
- the whole code with `--minify all`.

Not that the minification function is incomplete.
Unnecessary spaces may be inserted.

### `--rustfmt`

Formats the output with Rustfmt.

### `--check`

Creates a temporary package that shares the current target directory and execute `cargo check` before outputting.

This flag works even if bundling was skipped by `#![cfg_attr(cargo_equip, cargo_equip::skip)]`.

```console
❯ cargo equip --check -o /dev/null
     Running `/home/ryo/.cargo/bin/rustup run nightly cargo udeps --output json -p solve --bin solve`
    Checking solve v0.0.0 (/home/ryo/src/local/a/solve)
    Finished dev [unoptimized + debuginfo] target(s) in 0.13s
info: Loading save analysis from "/home/ryo/src/local/a/solve/target/debug/deps/save-analysis/solve-4eea33c8603d6001.json"
    Bundling the code
    Checking cargo-equip-check-output-6j2i3j3tgtugeaqm v0.1.0 (/tmp/cargo-equip-check-output-6j2i3j3tgtugeaqm)
    Finished dev [unoptimized + debuginfo] target(s) in 0.11s
```

## License

Dual-licensed under [MIT](https://opensource.org/licenses/MIT) or [Apache-2.0](http://www.apache.org/licenses/LICENSE-2.0).
