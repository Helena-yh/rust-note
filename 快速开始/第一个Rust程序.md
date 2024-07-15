# 第一个Rust程序

## 创建一个新的 Rust 项目
Rust 使用 Cargo 作为官方的构建工具和包管理器，下面通过 Cargo 创建一个新的 Rust 项目

```sh
cargo new Hello_world
```

其中 Cargo.toml 是项目的配置文件，src 为源代码目录，main.rs 为主程序文件。

```sh
├─src
├─.gitignore
├─Cargo.lock
└─Cargo.toml
```

然后我们可以使用 `cargo build` 命令进行项目编译，编译完毕后 Cargo 会自动生成如下的文件目录

```sh
├─src

├─target
    └─debug
        ├─.fingerprint
        │  └─Hello_world-f64f551fa382f811
        ├─build
        ├─deps
        ├─examples
        └─incremental
├─.gitignore
├─Cargo.lock
└─Cargo.toml
```

## 编写程序

默认情况下，main.rs文件中已经包含了一个简单的 hello_world 程序，其中，main 函数是 Rust 程序的入口；而 println! 则是一个Rust宏（而非函数），用于在标准输出上打印一行文本。代码如下：

```rs
fn main() {
    println!("Hello, world!");
}
```

## 编译运行

```rs
cargo run
```

```sh
Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.02s
     Running `target/debug/Hello_world`
Hello, world!
```

## Cargo 常用命令列表

| 命令                          | 描述                                                                  |
| ------------------------------ | ------------------------------------------------------------------- |
| `cargo init`                   | 在当前目录创建一个新的项目，使用最新的版本。                               |
| `cargo new name`               | 在新目录中创建一个新的 Rust 项目                                         |
| `cargo build`                  | 以调试模式构建项目（使用 `--release` 进行全部优化）。                      |
| `cargo check`                  | 检查项目是否能够编译通过（速度更快）。                                     |
| `cargo test`                   | 运行项目的测试。                                                       |
| `cargo run`                    | 运行项目的二进制文件（main.rs）。                                        |
| `cargo run --bin b`            | 运行二进制文件 `b`。与其他依赖项统一特性（可能会有些混淆）。                  |
| `cargo run -p w`               | 运行子工作区 `w` 的主程序。更符合预期的特性处理方式。                        |
| `cargo doc --open`             | 为你的代码和依赖项生成本地文档，并在浏览器中打开。                            |
| `rustup doc`                   | 打开离线的 Rust 文档（包括书籍），适用于在飞机上阅读！                        |

