# Shell 脚本和编程

## 基本概念

- 终端：硬件设备，处理输入计算输出
- tts：等价于终端
- 终端模拟器：软件，提供输入环境展示输出
- Shell：解析输入，将输入转化为内核能够识别的指令
- Bash：Shell 的一种实现，Linux 内置的标准 Shell

## 语法

### 变量

- 自定义变量使用 `=` 号声明，作用域为当前 shell
- 环境变量使用 `export`、`declare -x` 声明，作用域为当前 shell 与子 shell
- 系统环境变量，无需声明，内置启动加载

```bash
#!/bin/bash

# 变量名=变量值
page_num=1
page_size=10

# 复制命令，ls 命令，列举当前目录或指定目录所有文件，也可列举文件权限
_ls=ls

# 复制命令执行结果
file_list=$(ls -a)

# shell 中普通声明的变量都为字符串，无法执行算数运算
# let 命令用于指定算术运算，即 let expretion
# bash 中引用变量需要使用 ${变量名}，在 let 表达式中不用
let total=page_num*page_size

# 定义并声明变量类型为 int
# 语法：declare [+/-][rxi][变量名称＝设置值] 或 declare -f
declare -i total=page_num*page_size

# 声明环境变量
export total
declare -x total
```

### 运算符与引用

    <table style="text-align: center" border="1">
      <tr>
        <th>类型</th>
        <th>符号</th>
        <th>作用</th>
      </tr>
      <tr>
        <td>算数运算符</td>
        <td>+、-、*、/、|、&、%</td>
        <td>常规算数运算</td>
      </tr>
      <tr>
        <td>逻辑运算符</td>
        <td>||、&&、!</td>
        <td>逻辑运算</td>
      </tr>
      <tr>
        <td>比较运算符</td>
        <td>|>、&lt;、==、!=</td>
        <td>比较运算</td>
      </tr>
      <tr>
        <td rowspan="3">引号</td>
        <td>单引</td>
        <td>解决空格问题，单引中的所有内容按原样输出</td>
      </tr>
      <tr>
        <td>双引</td>
        <td>解决空格问题，能够解析 $(参数替换)、`(命令替换)</td>
      </tr>
      <tr>
        <td>反引</td>
        <td>解决空格问题，执行命令</td>
      </tr>
      <tr>
        <td rowspan="2">圆括号</td>
        <td>(())</td>
        <td>算数运算 foo=$((1+2))</td>
      </tr>
      <tr>
        <td>()</td>
        <td>执行命令 foo=$(ls -a)</td>
      </tr>
      <tr>
        <td rowspan="3">命令连接</td>
        <td>||</td>
        <td>命令 1 返回结果为 非 0(视为 false)，则继续执行命令 2</td>
      </tr>
      <tr>
        <td>&&</td>
        <td>命令 1 返回结果为 0(视为 true)，则继续执行命令 2</td>
      </tr>
      <tr>
        <td>;</td>
        <td>串行执行命令</td>
      </tr>
      <tr>
        <td>后台运行</td>
        <td>&</td>
        <td>命令在后台持续运行（shell 进程关闭命令停止运行）</td>
      </tr>
    </table>

### 管道

管道符 `|`，管道符连接左右两侧的程序，左侧的输出作为右侧的输入

### 重定向

| 命令            | 说明                                               |
| :-------------- | :------------------------------------------------- |
| command > file  | 将输出重定向到 file。                              |
| command 2> file | 将命令产生的错误重定向到 file                      |
| command &> file | 将输出和错误输出统一重定向到 file                  |
| command < file  | 将输入重定向到 file。                              |
| command >> file | 将输出以追加的方式重定向到 file。                  |
| n > file        | 将文件描述符为 n 的文件重定向到 file。             |
| n >> file       | 将文件描述符为 n 的文件以追加的方式重定向到 file。 |
| n >& m          | 将输出文件 m 和 n 合并。                           |
| n <& m          | 将输入文件 m 和 n 合并。                           |
| << tag          | 将开始标记 tag 和结束标记 tag 之间的内容作为输入。 |

### 判断

shell 提供三种判断符合，可用于：整数测试、字符串测试、文件测试

- test condition
- [condition]
- [[condition]]

```bash
#!/bin/bash

# 整数测试
# -lt 小于
test $num_1 -lt $num_2
# -eq 等于
test $num_1 -eq $num_2
# -gt 大于
test $num_1 -gt $num_2

# 字符串测试
# -z 是否为空
test -z $str_a
# -n 是否不为空
test -n $str_b
# = 是否相等
test $str_a = $str_b

# 文件测试
# -e 文件是否存在
test -e file_path
# -f 文件是否存在且是普通文件
test -f file_path
```

- `[]` 中括号比较前后要有空格，否则报错，在其中的变量建议使用 `""` 双引号包裹；

- `test` 与 `[]` 中的 `<`、`=`、`>` 号只能比较字符串；
- `[[]]` 中支持使用 `<`、`=`、`>` 比较整型，且支持使用 `=~` 正则匹配字符串

### 分支语句

```bash
#!/bin/bash

# if 分支，对应 if () {} else if () {} else {}
if condition; then
  # dosomething
elif condition; then
  # dosomething
else
  # dosomething
fi

# case 分支，对应 switch (variable) { case 'content_1': break; default: ; }
case $variable in
  "content_1")
    # dosomething
   ;;
   *)
     # dosomething
   ;;
esac
```

### 循环

```bash
#!/bin/bash

# while，对应 while (condition) {}
while condition;
do
  # dosomething
done

# until，与 while 相反，条件成立跳出循环
until condition;
do
  # dosomething
done

# for
# 列表循环
for foo in a b c
do
  # dosomething
done
# 计数循环
for((i=0; i< 10; i++))
do
  # dosomething
done
```

### 函数

```bash
#!/bin/bash

# 直接声明
functionName(){}

# 关键词声明
function name() {}

# 函数引用
# 直接引用函数名称，后跟参数列表，在函数内部使用 $1、$2... 引用参数
name args_1 args_2
```

- 函数必须先定义后使用
- 函数中 `$0` 指向函数名
- 函数中 `return` 关键字不返回结果，只表示函数执行状态
- 函数返回结果一般通过 `echo`、`printf` ，外部通过 `$()` 与反引号接收
- 函数没有显示 `return`，则函数执行状态是上一条命令的执行状态，储存于 `$?` 中
- 函数内使用 `local` 声明变量，将作用域限制在函数内部，函数执行完成建议使用 `unset` 释放变量

### 模块

```bash
#!/bin/bash

# 导入其他 .sh 脚本
source module_path
```

## Shell 执行过程和原理

### 执行

脚本第一行指定使用什么解释器来执行当前 shell 脚本。

#### 启动方式

shell 脚本有三种启动方式：

- 直接运行（`./filepath.sh`），需要有执行权限，子进程运行
- 解释器运行（`bash ./filepath.sh`），使用指定的解释器运行脚本，子进程运行
- source 运行（`source ./filepath.sh`），直接在当前进程运行程序

#### 执行流程

解释器解析 shell 脚本分为：

- 字符解析
  - 识别换行符、分号，行切割
  - 识别命令连接符，命令切割
  - 识别空格、tab 符，命令、参数切割
- shell 展开，如 `{1...3}` 展开为 `1 2 3`
- 重定向，将 stdin、stdout、stderr 的文件描述符的指向变更
- 执行命令
  - 内置命令在当前进程直接执行
  - 非内置命令在 `$path` 环境变量中查找，然后启动子进程执行
- 收集状态并返回

#### shell 展开

```bash
#!/bin/bash

# 大括号展开 {}
# 字符串序列
a{c,d,e}b # acb adb aeb
# 表达式序列
{1...5} # 1 2 3 4 5
{1...5...2} # 1 3 5
{a...e} # a b c d e

# 参数展开 ${}
# 空参数处理，为空替换
${variable:-value}
# 空参数处理，为空替换，并赋值到 $variable
${variable:=value}
# 空参数处理，为空报错
${variable:?value}
# 空参数处理，不为空替换
${variable:+value}
# 参数切片
${variable:start}
# 参数切片
${variable:start:length}
# 参数部分删除，最小限度从后截取 value
${variable%value}
# 参数部分删除，最大限度从后截取 value
${variable%%value}
# 参数部分删除，最小限度从前截取 value
${variable#value}
# 参数部分删除，最大限度从前截取 value
${variable##value}
```

### 调试

- log、echo、printf
- set 命令，一般在脚本开始设置 set 配置
- 插件

shell 编程基础：

- [shell 编程](https://shellscript.readthedocs.io/zh_CN/latest/)

> ByteTech
