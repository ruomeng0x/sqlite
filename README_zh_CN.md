# README

记录关于sqlite学习的过程，学习的版本基于sqlite 1.0.1, commmit id: `2d2fcdfe59779af1c2410fe6020cb7c81165f8b0`. 目前修复了代码中存在的问题，可以在macOS12.3 & Fedora 36中编译通过。

# Build

## Fedora 36

使用docker(或者实体机也行)编译的过程如下:

```bash
docker pull fedora:36
docker run -dt --name f36 --cap-add=SYS_PTRACE --security-opt seccomp=unconfined fedora:36
docker exec -it f36 bash
dnf update -y
dnf install -y ncurses git hostname clang diffutils readline-devel tcl-devel gdbm-devel bear
mkdir -p ~/local/src/c/sqlite && cd ~/local/src/c/sqlite
git clone https://github.com/ruomeng0x/sqlite.git
cd ~/local/src/c/sqlite/sqlite
git checkout study-1.x
mkdir build && cd build
CC=clang CFLAGS="-std=c89 -g3" bash ../configure
# 编译并生成compile_commands.json
bear -- make
```

## Ubuntu

```bash
sudo apt install -y bear make gcc libreadline-dev libgdbm-dev
mkdir -p ~/local/src/c/sqlite && cd ~/local/src/c/sqlite
git clone https://github.com/ruomeng0x/sqlite.git
cd ~/local/src/c/sqlite/sqlite
git checkout study-1.x
mkdir build && cd build
CC=clang CFLAGS="-std=c89 -g3" bash ../configure
# 编译并生成compile_commands.json
bear -- make
```


## macOS 

```bash
brew install gdbm readline
mkdir -p ~/local/src/c/sqlite && cd ~/local/src/c/sqlite
git clone https://github.com/ruomeng0x/sqlite.git
cd ~/local/src/c/sqlite/sqlite
git checkout study-1.x
mkdir build && cd build
CC=clang CFLAGS="-std=c89 -g3" bash ../configure

# 列出安装的目录，并记下版本号
brew list gdbm readline bear
# 修改Makefile中的这几行变量，将版本号改成上面获取到的值
GDBM_FLAGS = -I/opt/homebrew/Cellar/gdbm/1.23/include
LIBGDBM = -lgdbm -L/opt/homebrew/Cellar/gdbm/1.23/lib
READLINE_FLAGS = -DHAVE_READLINE=1 -I/opt/homebrew/Cellar/readline/8.1.2/include/readline
LIBREADLINE = -lreadline -L/opt/homebrew/Cellar/readline/8.1.2/lib

# 编译并生成compile_commands.json
bear -- make
```

# 其他

可以在github中查看原来的1.0.1附近的commit[历史](https://github.com/ruomeng0x/sqlite/commits/master?before=d6bb6000eff360f76d736d13331816354d880c5f+23931&branch=master)
