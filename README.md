# InfiniLM

## 基础运行说明

- 克隆仓库

```sh
$ git clone https://github.com/InfiniTensor/InfiniLM
```

- 拉取测试模型

```sh
$ cd InfiniLM

$ git lfs install

$ git clone https://huggingface.co/TinyLlama/TinyLlama-1.1B-Chat-v1.0
```

如果上面两步进行较慢，或者不支持 lfs，可以选择访问：https://huggingface.co/TinyLlama/TinyLlama-1.1B-Chat-v1.0/tree/main 并且手动下载 config.json, model.safetensors, tokenizer.model 三个文件，将其打包放入 `TinyLlama-1.1B-Chat-v1.0` 并且放入项目根目录

- 运行模型

```sh
$ cargo cast --model TinyLlama-1.1B-Chat-v1.0 --dt f16

$ cargo chat --model TinyLlama-1.1B-Chat-v1.0_F16
```

## 文件说明

- debug.S: 可执行文件的反汇编代码

  生成方式： 在 InfiniLM 项目根目录下执行以下命令
  ```sh
  $ cargo build --package xtask --release
  $ objdump -d -S target/release/xtask > debug.S
  ```

- command.log：使用 strace 分析系统调用时使用的指令和模型的输出内容

- `stdout.txt`：strace 分析模型调用的 syscall 内容

- `stderr.txt`：模型运行过程中的 stderr 流输出

- `syscall_set`：对 `stdout.txt` 中涉及的 syscall 进行总结去重得到的列表


## 内核运行
- 内核地址： [https://github.com/Starry-OS/Starry](StarryOS)

- 内核运行方式：在内核根目录下执行以下命令
```sh
$ git checkout feat_InfiniLM
$ ./build_img.sh -file x86_64_InfiniLM
$ make A=apps/monolithic_userboot LOG=error ARCH=x86_64 ACCEL=n run
```