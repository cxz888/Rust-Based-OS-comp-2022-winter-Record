# Rust-Based OS comp 2022 winter Record

记录参与 2022 秋冬季开源操作系统训练营的过程。

## 2022-10-28

做完 rustlings。

我学习 Rust 已经有一段时间了。之前只是听说过 rustlings 但没做过。

因为比较习惯本地开发，所以就记录一下 VSCode+Win11 要做的事吧。

其实也很简单，照着 [scheduling](https://github.com/LearningOS/rust-based-os-comp2022/blob/main/scheduling.md#step-0-%E8%87%AA%E5%AD%A6rust%E7%BC%96%E7%A8%8B%E5%A4%A7%E7%BA%A6714%E5%A4%A9) 里的提示创建好练习用的 repo 之后，用 ssh 方式下载到本地。记得配置好公钥。

然后在仓库根目录 `cargo install --force --path .` 一下，好了之后 `make setupclassroom` 初始化 CI。

现在就可以 `rustlings watch` 开始做题了，仔细看终端输出的提示即可。

另外，如果还没全做完就 `git push` 到了远端，CI 暂时无法通过，所以会显示 `Points 0/100`。（吐槽一句 CI 真是有够慢的）

> 2022-11-5：现在已经可以部分评分了

## 2022-10-29

过了一遍《计算机组成与设计（RISC-V版）》一、二章。

尝试还是用 Win11 开发 rCore。至少 lab0-0 能跑啦，至于后续要是遇到什么问题再说吧 (￣▽￣)~*。

总结一下步骤：

1. 准备 qemu 环境，可以通过 scoop 安装，也可以直接去[官网](https://qemu.weilnetz.de/w64/2022/)下载最新的安装包，安装好后可以用 `qemu-system-riscv64 --version` 看看是否成功。
2. 注意看根目录下的 rust-toolchain.toml，它里面指定了使用 nightly-2022-08-05 的版本。然后如果 rustup 用了镜像源，镜像源那里可能只有近几个月的版本，就会提示找不到该版本。切回官方源就好了。
3. 然后直接在 os1 目录运行 `$env:LOG="TRACE";make run`，等着依赖安装好，应该就能正常显示结果了。

## 2022-10-30

RISC-V 手册读了大半。

## 2022-10-31

RISC-V 手册读完（第八章向量架构没有仔细读）

RISC-V 特权架构过了一遍。

新概念好多，有点晕。

## 2022-11-1

学习清华操作系统课程一二讲。

## 2022-11-2

学习清华操作系统课程第三四次课。

尝试 lab0-1。

## 2022-11-3

学习清华操作系统课程第五次课。

大致理解 lab0-1 流程。

尝试 lab1。

## 2022-11-4

lab1 差不多解决了。

似乎是出现了静态变量太大，溢出到代码段的问题，导致出现了非常玄学的表现。

可能需要把 `TaskInfo` 存放的位置改一下。但我发现把 `opt-level=0` 注释掉，再打开 `lto="fat"` 就可以过。

先过了再说吧。没有虚拟内存管理还真是不方便。

2022-11-11 补充：出现这个问题的根本原因是，我将 `TaskInfo` 相关的信息，主要是一个用于统计调用次数的 `[u32;500]` 放在了 TCB 中，而 TCB 被包含在静态变量 `TASK_MANAGER` 中。它是由 `lazy_static!` 进行初始化的，在初始化的过程中就会把这些巨大的数组放到栈上过一遍，再放入 .data 或 .bss 段中——这个栈是系统初始化用的那个栈，也就是 `entry.asm` 中的 `boot_stack`，将这个栈增大就可以解决问题，但仍然要记得把优化等级 `opt-level=0` 注释掉，让编译器尽量优化

## 2022-11-5

通过 lab1。学习清华 OS 第六次课。看 lab2。

目前我还是用的 Windows 开发。写代码本身没有问题。但是 rCore 框架提供的很多测试脚本看上去都是用的 Linux 特有的命令，所以本地测试不太方便。这个指的是 `make test3` 这些，`make run` 还是基本上正常的。后面也不知道会不会遇到什么平台相关的坑呢。

~~Windows 没人权 XD~~

## 2022-11-6

看 lab2。

今天效率有点低，可能是睡少了？精神状态不佳……

## 2022-11-7

总算解决了 lab2。

收获良多，~~感觉我整个人快变成一张页表了~~。

稍微转移一下注意力，做一些别的大作业去。

## 2022-11-8

做别的大作业~

但还是稍微看了点清华 OS 第六讲的课件。

## 2022-11-9

看清华 OS 第八次课。

## 2022-11-10

看清华 OS 第九次课。看一部分 lab3

## 2022-11-11

看 lab3。准备开始做。

## 2022-11-12

完成 lab3。看了部分清华 OS 第十次课。

做 lab3 遇到了奇怪的问题。`make run BASE=0` 和 `make run BASE=1` 都好使，轮到 `make run BASE=2` 却在调用 `rust-objcopy` 时出了错，显示 `llvm-objcopy.exe: error: permission denied`。看了看结果，到一半的时候失败了。和群里的各位讨论一番无果，只好手动构建好用户应用。也许是 windows 的坑？

## 2022-11-13

看清华 OS 第十一次课。读 lab4。

## 2022-11-14

看 lab4。今天在做另一个大作业，所以花的时间不多。

## 2022-11-15~11-16

两天都在钻研 lab4，这边内容不少。

后来还遇到了非常奇怪的 bug，排了几个小时才发现是一个极其简单的逻辑错误。

底层的 bug 溯源起来十分困难呢。

因为睡太晚，一直感觉效率低下，必须要早睡了。

## 2022-11-17

做完了 lab4。

## 2022-11-18

看清华 OS 第十一次课。

## 2022-11-19~11-20

这两天在做另一门比较紧的大作业。

## 2022-11-21~11-23

好多大作业。

看清华 OS 第十二次课。

## 2022-11-24~11-30

折腾这么多天，总算基本整完了大作业。

OS 第十三次课看完了。但不知道为什么后面没有了。

跟着 tutorial 做了 OS7，准备做 OS8 了，听说内容很多。

## 2022-12-1~12-3

做完了 OS8，^_^。

可以写总结报告了。

开始研究 fat32 文件系统。

## 2022-12-4~12-5

这两天学习了一下 fat32

## 2022-12-6~12-7

将之前实验的代码整理。

## 2022-12-8~2023-1-3

这段时间又在忙某门课的作业。临近期末了，有点忙碌。

总算忙完了学校的所有事。

## 2023-1-4

今天在考虑文件系统的实现。
