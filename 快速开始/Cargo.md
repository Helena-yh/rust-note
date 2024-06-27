# Cargo

> [Cargo Guide](http://doc.crates.io/guide.html)

[Cargo](http://doc.crates.io/) 是 Rust 内置的代码组织管理工具，它借鉴了 Maven, Gradle, Go, Npm 等依赖管理、项目构建工具的优点，提供了一系列的工具，从项目的建立、构建到测试、运行直至部署，为 Rust 项目的管理提供尽可能完整的手段。Cargo 目前包含在了官方的分发包中，Rust 开发环境安装完毕后，我们可以直接在命令行中使用 `cargo` 命令来创建新的项目：

```sh
# --bin 表示该项目将生成可执行文件，否则表示生成库文件
$ cargo new hello-rust --bin
```

`cargo new` 指令会在当前目录下新建了基于 cargo 项目管理的 Rust 项目，我们自动生成了基本运行所必须的所有代码：

```rs
fn main() {
    println!("Hello, world!");
}
```

然后我们可以使用 `cargo build` 命令进行项目编译，编译完毕后 Cargo 会自动生成如下的文件目录

```sh
├─src
└─target
    └─debug
        ├─.fingerprint
        │  └─hello-rust-af2f81aa8cdfcf12
        ├─build
        ├─deps
        ├─examples
        ├─incremental
        └─native
```

最后使用 `cargo run` 即自动运行生成的代码：

```sh
Finished dev [unoptimized + debuginfo] target(s) in 0.0 secs
    Running `target\debug\hello-rust.exe`
Hello, world!
```

如果我们希望生成发布包，则可以使用 `cargo build --release` 来进行带优化操作的编译：

```sh
$ cargo build --release
   Compiling hello-rust v0.1.0 (file:///path/to/project/hello-rust)
```

对于现有的 Cargo 项目，我们同样可以直接利用 build 指令进行编译：

```sh
$ git clone https://github.com/rust-lang-nursery/rand.git
$ cd rand

$ cargo build
   Compiling rand v0.1.0 (file:///path/to/project/rand)
```

完整的 Cargo 命令列表如下：

| Command                        | Description                                                                |
| ------------------------------ | -------------------------------------------------------------------------- |
| `cargo init`                   | Create a new project for the latest edition.                               |
| `cargo build`                  | Build the project in debug mode (`--release` for all optimization).        |
| `cargo check`                  | Check if project would compile (much faster).                              |
| `cargo test`                   | Run tests for the project.                                                 |
| `cargo run`                    | Run your project, if a binary is produced (main.rs).                       |
| `cargo run --bin b`            | Run binary `b`. Unifies features with other dependents (can be confusing). |
| `cargo run -p w`               | Run main of sub-workspace `w`. Treats features more as you would expect.   |
| `cargo doc --open`             | Locally generate documentation for your code and dependencies.             |
| `cargo rustc -- -Zunpretty=X`  | Show more desugared Rust code, in particular with X being:                 |
| `expanded`                     | Show with expanded macros, ...                                             |
| `cargo +{nightly, stable} ...` | Runs command with given toolchain, e.g., for 'nightly only' tools.         |
| `rustup doc`                   | Open offline Rust documentation (incl. the books), good on a plane!        |


## cargo.toml 与 cargo.lock

cargo.toml 和 cargo.lock 文件总是位于项目根目录下。

- 源代码位于 src 目录下。
- 默认的库入口文件是 src/lib.rs。
- 默认的可执行程序入口文件是 src/main.rs。
- 其他可选的可执行文件位于 `src/bin/*.rs`(这里每一个 rs 文件均对应一个可执行文件)。
- 外部测试源代码文件位于 tests 目录下。
- 示例程序源代码文件位于 examples。
- 基准测试源代码文件位于 benches 目录下。

cargo.toml 和 cargo.lock 是 cargo 项目代码管理的核心两个文件，cargo 工具的所有活动均基于这两个文件。

cargo.toml 是 cargo 特有的项目数据描述文件，cargo.toml 文件存储了项目的所有信息，如果想让自己的 rust 项目能够按照期望的方式进行构建、测试和运行，那么，必须按照合理的方式构建'cargo.toml'。而 cargo.lock 文件则不直接面向开发中，也不需要直接去修改这个文件。lock 文件是 cargo 工具根据同一项目的 toml 文件生成的项目依赖详细清单文件，所以我们一般不用不管他，只需要对着 cargo.toml 文件撸就行了。


# 依赖管理

Cargo 的依赖管理主要依托于 Cargo.toml 与 Cargo.lock 这两个文件，Cargo.toml 文件存储了项目的所有信息；Cargo.lock 则是根据同一项目的 toml 文件生成的项目依赖详细清单文件，类似于 yarn.lock，其可以避免泛版本号带来的依赖版本混乱问题。Cargo 使用了集中式地包管理 [crates.io](https://crates.io/)，我们可以在上面搜索需要的第三方依赖，并将其添加到 Cargo.toml 中

```sh
[dependencies]
time = "0.1.12"
regex = "0.1.41"
```