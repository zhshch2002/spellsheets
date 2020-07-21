---
title: Bash scripting
category: CLI
layout: sheet
tags: [Featured]
updated: 2020-07-05
keywords:
  - Variables
  - Functions
  - Interpolation
  - Brace expansions
  - Loops
  - Conditional execution
  - Command substitution
---

Getting started
---------------
{: .-three-column}

### 介绍
{: .-intro}

开始学习 Bash 的参考资料。

- [Learn bash in y minutes](https://learnxinyminutes.com/docs/bash/) _(learnxinyminutes.com)_
- [Bash Guide](http://mywiki.wooledge.org/BashGuide) _(mywiki.wooledge.org)_

### 例子

```bash
#!/usr/bin/env bash

NAME="John"
echo "Hello $NAME!"
```

### 变量

```bash
NAME="John"
echo $NAME
echo "$NAME"
echo "${NAME}!"
```

### 字符串引用

```bash
NAME="John"
echo "Hi $NAME"  #=> Hi John
echo 'Hi $NAME'  #=> Hi $NAME
```

### 执行 Shell

```bash
echo "I'm in $(pwd)"
echo "I'm in `pwd`"
# Same
```

See [Command substitution](http://wiki.bash-hackers.org/syntax/expansion/cmdsubst)

### 条件执行

```bash
git commit && git push
git commit || echo "Commit failed"
```

### 函数
{: id='functions-example'}

```bash
get_name() {
  echo "John"
}

echo "You are $(get_name)"
```

See: [函数](#函数)

### 判断
{: id='conditionals-example'}

```bash
if [[ -z "$string" ]]; then
  echo "String is empty"
elif [[ -n "$string" ]]; then
  echo "String is not empty"
fi
```

See: [判断](#判断)

### Strict mode

```bash
set -euo pipefail
IFS=$'\n\t'
```

See: [Unofficial bash strict mode](http://redsymbol.net/articles/unofficial-bash-strict-mode/)

### 花括号扩展

```bash
echo {A,B}.js
```

| Expression | Description         |
| ---------- | ------------------- |
| `{A,B}`    | Same as `A B`       |
| `{A,B}.js` | Same as `A.js B.js` |
| `{1..5}`   | Same as `1 2 3 4 5` |

See: [Brace expansion](http://wiki.bash-hackers.org/syntax/expansion/brace)

参数扩展
--------------------
{: .-three-column}

### Basics

```bash
name="John"
echo ${name}
echo ${name/J/j}    #=> "john" （替换）
echo ${name:0:2}    #=> "Jo" （截取）
echo ${name::2}     #=> "Jo" （截取）
echo ${name::-1}    #=> "Joh" （截取）
echo ${name:(-1)}   #=> "n" （从右截取）
echo ${name:(-2):1} #=> "h" （从右截取）
echo ${food:-Cake}  #=> $food or "Cake"
```

```bash
length=2
echo ${name:0:length}  #=> "Jo"
```

See: [Parameter expansion](http://wiki.bash-hackers.org/syntax/pe)

```bash
STR="/path/to/foo.cpp"
echo ${STR%.cpp}    # /path/to/foo
echo ${STR%.cpp}.o  # /path/to/foo.o
echo ${STR%/*}      # /path/to

echo ${STR##*.}     # cpp (extension)
echo ${STR##*/}     # foo.cpp (basepath)

echo ${STR#*/}      # path/to/foo.cpp
echo ${STR##*/}     # foo.cpp

echo ${STR/foo/bar} # /path/to/bar.cpp
```

```bash
STR="Hello world"
echo ${STR:6:5}   # "world"
echo ${STR: -5:5}  # "world"
```

```bash
SRC="/path/to/foo.cpp"
BASE=${SRC##*/}   #=> "foo.cpp" (basepath)
DIR=${SRC%$BASE}  #=> "/path/to/" (dirpath)
```

### 替换

| Code              | Description          |
| ----------------- | -------------------- |
| `${FOO%suffix}`   | 移除后缀 (suffix)    |
| `${FOO#prefix}`   | 移除前缀 (prefix)    |
| ---               | ---                  |
| `${FOO%%suffix}`  | 移除 suffix 后的文本 |
| `${FOO##prefix}`  | 移除 prefix 前的文本 |
| ---               | ---                  |
| `${FOO/from/to}`  | 一次替换             |
| `${FOO//from/to}` | 全部替换             |
| ---               | ---                  |
| `${FOO/%from/to}` | 替换前缀             |
| `${FOO/#from/to}` | 替换后缀             |

### 注释

```bash
# 单行注释
```

```bash
: '
这是
一个
多行注释
'
```

### 子字符串

| Expression      | Description                |
| --------------- | -------------------------- |
| `${FOO:0:3}`    | 截取第 0 到 3 个之间的字符 |
| `${FOO:(-3):3}` | 从右侧截取                 |

### 长度

| Expression | Description      |
| ---------- | ---------------- |
| `${#FOO}`  | 取 `$FOO` 的长度 |

### 操作

```bash
STR="HELLO WORLD!"
echo ${STR,}   #=> "hELLO WORLD!" （小写第一个字母）
echo ${STR,,}  #=> "hello world!" （全部小写）

STR="hello world!"
echo ${STR^}   #=> "Hello world!" （大写第一个字母）
echo ${STR^^}  #=> "HELLO WORLD!" （全部大写）
```

### 默认值

| Expression        | Description                                              |
| ----------------- | -------------------------------------------------------- |
| `${FOO:-val}`     | `$FOO`, or `val` if unset (or null)                      |
| `${FOO:=val}`     | Set `$FOO` to `val` if unset (or null)                   |
| `${FOO:+val}`     | `val` if `$FOO` is set (and not null)                    |
| `${FOO:?message}` | Show error message and exit if `$FOO` is unset (or null) |

省略`:`将取消无效性检查，比如 `${FOO-val}`在 `$FOO`未设置时 (unset) 表示 `val`

循环
-----
{: .-three-column}

### Basic for loop

```bash
for i in /etc/rc.*; do
  echo $i
done
```

### C-like for loop

```bash
for ((i = 0 ; i < 100 ; i++)); do
  echo $i
done
```

### Ranges

```bash
for i in {1..5}; do
    echo "Welcome $i"
done
```

#### 设置步长

```bash
for i in {5..50..5}; do
    echo "Welcome $i"
done
```

### 以行读取

```bash
cat file.txt | while read line; do
  echo $line
done
```

### 死循环

```bash
while true; do
  ···
done
```

函数
---------
{: .-three-column}

### 定义函数

```bash
myfunc() {
    echo "hello $1"
}
```

```bash
# Same as above (alternate syntax)
function myfunc() {
    echo "hello $1"
}
```

```bash
myfunc "John"
```

### 返回数值

```bash
myfunc() {
    local myresult='some value'
    echo $myresult
}
```

```bash
result="$(myfunc)"
```

### 报错

```bash
myfunc() {
  return 1
}
```

```bash
if myfunc; then
  echo "success"
else
  echo "failure"
fi
```

### 参数

| Expression | Description              |
| ---------- | ------------------------ |
| `$#`       | 第#个参数                |
| `$*`       | 全部参数                 |
| `$@`       | 从第一个开始的全部参数   |
| `$1`       | 第一个参数               |
| `$_`       | 上一个指令的最后一个参数 |

See [Special parameters](http://wiki.bash-hackers.org/syntax/shellvars#special_parameters_and_shell_variables).

判断
------------
{: .-three-column}

### Conditions

提示， `[[` 实践上是一个返回`0`(true) 或者`1`(false) 的指令，任何遵从同样逻辑的程序，例如`grep(1)` 和 `ping(1)`都可以用来作为条件判断。

| Condition                | Description  |
| ------------------------ | ------------ |
| `[[ -z STRING ]]`        | 字符串为空   |
| `[[ -n STRING ]]`        | 字符串不为空 |
| `[[ STRING == STRING ]]` | 相等         |
| `[[ STRING != STRING ]]` | 不等         |
| ---                      | ---          |
| `[[ NUM -eq NUM ]]`      | 相等         |
| `[[ NUM -ne NUM ]]`      | 不等         |
| `[[ NUM -lt NUM ]]`      | 小于         |
| `[[ NUM -le NUM ]]`      | 小于等于     |
| `[[ NUM -gt NUM ]]`      | 大于         |
| `[[ NUM -ge NUM ]]`      | 大于等于     |
| ---                      | ---          |
| `[[ STRING =~ STRING ]]` | 正则         |
| ---                      | ---          |
| `(( NUM < NUM ))`        | 数值条件     |

#### 更多

| Condition            | Description |
| -------------------- | ----------- |
| `[[ -o noclobber ]]` | 当选项使能  |
| ---                  | ---         |
| `[[ ! EXPR ]]`       | Not         |
| `[[ X && Y ]]`       | And         |
| `[[ X || Y ]]`       | Or          |

### 文件条件

| Condition               | Description        |
| ----------------------- | ------------------ |
| `[[ -e FILE ]]`         | 存在               |
| `[[ -r FILE ]]`         | 可读               |
| `[[ -h FILE ]]`         | Symlink            |
| `[[ -d FILE ]]`         | 为目录             |
| `[[ -w FILE ]]`         | 可写               |
| `[[ -s FILE ]]`         | 文件大小 > 0 bytes |
| `[[ -f FILE ]]`         | 为文件             |
| `[[ -x FILE ]]`         | 可执行             |
| ---                     | ---                |
| `[[ FILE1 -nt FILE2 ]]` | 1 比 2 更新        |
| `[[ FILE1 -ot FILE2 ]]` | 2 比 1 更新        |
| `[[ FILE1 -ef FILE2 ]]` | 相同               |

### 例子

```bash
# String
if [[ -z "$string" ]]; then
  echo "String is empty"
elif [[ -n "$string" ]]; then
  echo "String is not empty"
else
  echo "This never happens"
fi
```

```bash
# Combinations
if [[ X && Y ]]; then
  ...
fi
```

```bash
# Equal
if [[ "$A" == "$B" ]]
```

```bash
# Regex
if [[ "A" =~ . ]]
```

```bash
if (( $a < $b )); then
   echo "$a is smaller than $b"
fi
```

```bash
if [[ -e "file.txt" ]]; then
  echo "file exists"
fi
```

数组
------

### 定义数组

```bash
Fruits=('Apple' 'Banana' 'Orange')
```

```bash
Fruits[0]="Apple"
Fruits[1]="Banana"
Fruits[2]="Orange"
```

### 使用

```bash
echo ${Fruits[0]}           # Element #0
echo ${Fruits[-1]}          # Last element
echo ${Fruits[@]}           # All elements, space-separated
echo ${#Fruits[@]}          # Number of elements
echo ${#Fruits}             # String length of the 1st element
echo ${#Fruits[3]}          # String length of the Nth element
echo ${Fruits[@]:3:2}       # Range (from position 3, length 2)
echo ${!Fruits[@]}          # Keys of all elements, space-separated
```

### 操作

```bash
Fruits=("${Fruits[@]}" "Watermelon")    # Push
Fruits+=('Watermelon')                  # Also Push
Fruits=( ${Fruits[@]/Ap*/} )            # Remove by regex match
unset Fruits[2]                         # Remove one item
Fruits=("${Fruits[@]}")                 # Duplicate
Fruits=("${Fruits[@]}" "${Veggies[@]}") # Concatenate
lines=(`cat "logfile"`)                 # Read from file
```

### 迭代

```bash
for i in "${arrayName[@]}"; do
  echo $i
done
```

字典
------------
{: .-three-column}

### 定义字典

```bash
declare -A sounds
```

```bash
sounds[dog]="bark"
sounds[cow]="moo"
sounds[bird]="tweet"
sounds[wolf]="howl"
```

声明`sound`为 Dictionary 对象（又称关联数组）。

### 使用

```bash
echo ${sounds[dog]} # Dog's sound
echo ${sounds[@]}   # All values
echo ${!sounds[@]}  # All keys
echo ${#sounds[@]}  # Number of elements
unset sounds[dog]   # Delete dog
```

### 迭代

#### 值

```bash
for val in "${sounds[@]}"; do
  echo $val
done
```

#### 键

```bash
for key in "${!sounds[@]}"; do
  echo $key
done
```

选项
-------

### 选项

```bash
set -o noclobber  # Avoid overlay files (echo "hi" > foo)
set -o errexit    # Used to exit upon error, avoiding cascading errors
set -o pipefail   # Unveils hidden failures
set -o nounset    # Exposes unset variables
```

### Glob 选项

```bash
shopt -s nullglob    # Non-matching globs are removed  ('*.foo' => '')
shopt -s failglob    # Non-matching globs throw errors
shopt -s nocaseglob  # Case insensitive globs
shopt -s dotglob     # Wildcards match dotfiles ("*.sh" => ".foo.sh")
shopt -s globstar    # Allow ** for recursive matches ('lib/**/*.rb' => 'lib/a/b/c.rb')
```

Set `GLOBIGNORE` as a colon-separated list of patterns to be removed from glob
matches.

历史
-------

### 指令

| Command               | Description          |
| --------------------- | -------------------- |
| `history`             | 打印历史             |
| `shopt -s histverify` | 不要立即执行扩展结果 |

### 扩展

| Expression   | Description                    |
| ------------ | ------------------------------ |
| `!$`         | 展开最新命令的最后一个参数     |
| `!*`         | 展开最新命令的最后全部参数     |
| `!-n`        | 展开最新命令的第`n`个参数      |
| `!n`         | 展开历史命令的第`n`个          |
| `!<command>` | 展开最常调用的 `<command>`命令 |

### 操作

| Code                 | Description                                    |
| -------------------- | ---------------------------------------------- |
| `!!`                 | 再次执行上一条命令                             |
| `!!:s/<FROM>/<TO>/`  | 在最新命令中将第一次出现的`<FROM>`替换为`<TO>` |
| `!!:gs/<FROM>/<TO>/` | 在最新命令中将所有出现的`<FROM>`替换为`<TO>`   |
| `!$:t`               | 仅从最新命令的最后一个参数扩展基本名称         |
| `!$:h`               | 仅从最新命令的最后一个参数扩展目录             |

`!!`和`!$`可以替换为任何有效的扩展名。

### 截取

| Code     | Description                                                         |
| -------- | ------------------------------------------------------------------- |
| `!!:n`   | 从最近执行的命令只展开第`n`个 token（指令是 `0`; 第一个参数是 `1`） |
| `!^`     | 展开最新命令中的第一个参数                                          |
| `!$`     | 展开最近命令中的最后一个令牌                                        |
| `!!:n-m` | 从最新命令扩展令牌范围                                              |
| `!!:n-$` | 将第 n 个 token 扩展到最新命令中的最后一个                          |

`!!` can be replaced with any valid expansion i.e. `!cat`, `!-2`, `!42`, etc.

杂项
-------------

### 数值计算

```bash
$((a + 200))      # Add 200 to $a
```

```bash
$((RANDOM%=200))  # Random number 0..200
```

### 子命令行

```bash
(cd somedir; echo "I'm now in $PWD")
pwd # 不受上一个 cd 影响 依旧在原始目录
```

### 输出重定向

```bash
python hello.py > output.txt   # stdout to (file)
python hello.py >> output.txt  # stdout to (file), 追加
python hello.py 2> error.log   # stderr to (file)
python hello.py 2>&1           # stderr to stdout
python hello.py 2>/dev/null    # stderr to (null)
python hello.py &>/dev/null    # stdout and stderr to (null)
```

```bash
python hello.py < foo.txt      # feed foo.txt to stdin for python
```

### 检查命令

```bash
command -V cd
#=> "cd is a function/alias/whatever"
```

### 错误处理

```bash
trap 'echo Error at about $LINENO' ERR
```

or

```bash
traperr() {
  echo "ERROR: ${BASH_SOURCE[1]} at about ${BASH_LINENO[0]}"
}

set -o errtrace
trap traperr ERR
```

### Case/switch

```bash
case "$1" in
  start | up)
    vagrant up
    ;;

  *)
    echo "Usage: $0 {start|stop|ssh}"
    ;;
esac
```

### Source relative

```bash
source "${0%/*}/../share/foo.sh"
```

### printf

```bash
printf "Hello %s, I'm %s" Sven Olga
#=> "Hello Sven, I'm Olga

printf "1 + 1 = %d" 2
#=> "1 + 1 = 2"

printf "This is how you print a float: %f" 2
#=> "This is how you print a float: 2.000000"
```

### 目录脚本

```bash
DIR="${0%/*}"
```

### 获取选项

```bash
while [[ "$1" =~ ^- && ! "$1" == "--" ]]; do case $1 in
  -V | --version )
    echo $version
    exit
    ;;
  -s | --string )
    shift; string=$1
    ;;
  -f | --flag )
    flag=1
    ;;
esac; shift; done
if [[ "$1" == '--' ]]; then shift; fi
```

### 多行输入

```sh
cat <<END
hello world
END
```

### 读取输入

```bash
echo -n "Proceed? [y/n]: "
read ans
echo $ans
```

```bash
read -n 1 ans    # Just one character
```

### 特殊变量

| Expression | Description                  |
| ---------- | ---------------------------- |
| `$?`       | Exit status of last task     |
| `$!`       | PID of last background task  |
| `$$`       | PID of shell                 |
| `$0`       | Filename of the shell script |

See [Special parameters](http://wiki.bash-hackers.org/syntax/shellvars#special_parameters_and_shell_variables).

### 回到上一个目录

```bash
pwd # /home/user/foo
cd bar/
pwd # /home/user/foo/bar
cd -
pwd # /home/user/foo
```

### 判断命令的输出

```bash
if ping -c 1 google.com; then
  echo "It appears you have a working internet connection"
fi
```

### Grep check

```bash
if grep -q 'foo' ~/.bash_history; then
  echo "You appear to have typed 'foo' in the past"
fi
```

## Also see
{: .-one-column}

* [Bash-hackers wiki](http://wiki.bash-hackers.org/) _(bash-hackers.org)_
* [Shell vars](http://wiki.bash-hackers.org/syntax/shellvars) _(bash-hackers.org)_
* [Learn bash in y minutes](https://learnxinyminutes.com/docs/bash/) _(learnxinyminutes.com)_
* [Bash Guide](http://mywiki.wooledge.org/BashGuide) _(mywiki.wooledge.org)_
* [ShellCheck](https://www.shellcheck.net/) _(shellcheck.net)_
