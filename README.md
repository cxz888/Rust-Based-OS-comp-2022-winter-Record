# Rust-Based OS comp 2022 winter Record

记录参与 2022 秋冬季开源操作系统训练营的过程。

## 2022-10-28

开始做 rustlings。

本人已经学习 Rust 有一段时间了。之前只是听说过 rustlings 但没做过。

因为比较习惯本地开发，所以就记录一下 VSCode+Win11 要做的事吧。

其实也很简单，照着 scheduling 里的提示创建好 练习用的 repo 之后，用 ssh 方式下载到本地。记得配置好公钥。

然后在仓库根目录 `cargo install --force --path .` 一下，好了之后 `make setupclassroom` 初始化 CI。

现在就可以 `rustlings watch` 开始做题了，仔细看终端输出的提示即可。

另外，如果还没全做完就 `git push` 到了远端，CI 暂时无法通过，所以会显示 `Points 0/100`。（吐槽一句 CI 真是有够慢的）





