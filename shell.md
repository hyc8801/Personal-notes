# shell 笔记

<!-- TOC -->

- [shell 笔记](#shell-笔记)
  - [变量](#变量)
    - [定义变量，`变量名=变量值`](#定义变量变量名变量值)
    - [变量使用](#变量使用)
    - [只读变量](#只读变量)
    - [删除变量](#删除变量)
    - [变量类型](#变量类型)
  - [Shell 字符串](#shell-字符串)
    - [单引号](#单引号)
    - [双引号](#双引号)
    - [字符串拼接](#字符串拼接)
    - [字符串方法](#字符串方法)
  - [数组](#数组)
    - [定义数组](#定义数组)
    - [数组方法](#数组方法)
    - [注释](#注释)
  - [shell 参数传递](#shell-参数传递)
  - [shell 基本运算符](#shell-基本运算符)
    - [关系运算符](#关系运算符)
    - [布尔运算符](#布尔运算符)
    - [逻辑运算符](#逻辑运算符)
    - [字符串运算符](#字符串运算符)
    - [文件测试运算符](#文件测试运算符)
  - [echo 命令](#echo-命令)
    - [-e 开始转义](#-e-开始转义)
    - [显示命令执行结果](#显示命令执行结果)
  - [printf 命令](#printf-命令)
  - [test 命令](#test-命令)
  - [流程控制](#流程控制)
    - [if else 语句](#if-else-语句)
    - [for 循环](#for-循环)
    - [while 语句](#while-语句)
    - [until 循环](#until-循环)
    - [case 语句](#case-语句)
    - [break 命令](#break-命令)
    - [continue](#continue)
  - [Shell 函数](#shell-函数)
    - [函数参数](#函数参数)
  - [Shell 输入/输出重定向](#shell-输入输出重定向)
  - [Shell 文件包含（引入外部脚本）](#shell-文件包含引入外部脚本)

<!-- /TOC -->

## 变量

### 定义变量，`变量名=变量值`

**注意**: “=”中间不能有空格

```
name="yongchuan"
```

### 变量使用

```
# 使用一个定义过的变量，只要在变量名前面加美元符号即可
echo $name

# 变量名外面的花括号是可选的，加不加都行，加花括号是为了帮助解释器识别变量的边界
echo "my name is huang${name}"

#  重新赋值
your_name="tom"
echo $your_name
your_name="alibaba"
echo $your_name
```

### 只读变量

使用 readonly 命令可以将变量定义为只读变量，只读变量的值不能被改变。

```
myUrl="https://www.google.com"
readonly myUrl
```

### 删除变量

使用 unset 命令可以删除变量

```
name="huwenchun"
unset name
```

### 变量类型

运行 shell 时，会同时存在三种变量

- 局部变量
- 环境变量
- shell 变量
  > shell 变量是由 shell 程序设置的特殊变量。shell 变量中有一部分是环境变量，有一部分是局部变量，这些变量保证了 shell 的正常运行

## Shell 字符串

字符串是 shell 变成中最常用最有用的数据类型

### 单引号

```
str='this is a string'
```

单引号字符串的限制：

- 单引号里的任何字符都会原样输出，单引号字符串的变量是无效的；
- 单引号字符串不能出现单独的一个单引号（对单引号使用转义符后也不行，报错），但可成对出现，作为字符串拼接使用。

### 双引号

```
name="yongchuan"
str="my name is huang${name}"
echo -e str
```

双引号优点：

- 双引号里可以有变量
- 双引号里可以出现转义字符

### 字符串拼接

```
name="yongchuan"
# 双引号拼接
str1="hello, ${name}"
str2="hello, "${name}""

echo str1 str2

# 单引号拼接
str3='hello,${name}'
str3='hello,'$name''
```

### 字符串方法

```
# 获取字符串长度
string="abcdef bjkmk"
echo ${#string}

# 截取子字符串
echo ${string:1:3}

# 查找子字符串
echo `expr index "$string" bk`
```

## 数组

bash 支持一维数组（不支持多维数组），并且没有限定数组的大小。

### 定义数组

在 shell 中，用括号来表示数组，数组元素用“空格”符号隔开。

```
# 一般格式     数组名=(值1 值2 值3 值4 值5 )
array=(1 2 3 4 5 6)

# 单独定义数组的各个分量
array[0]=100
```

### 数组方法

```
array=(1 2 3 4 5 6)

# 读取数组的一般格式 ${数组名[下标]}
echo ${array[2]}

# 使用 @ 符号可以获取数组中的所有元素
echo ${array[@]}

# 读取数组长度（2中方法）
length=${#array[@]}
length2=${#array[*]}

```

### 注释

- 单行注释：以 # 开头的行就是注释，会被解释器忽略。
- 多行注释： `:<<EOF EOF` 以`:<<EOF`开头 ，以`EOF`结束 EOF 也可以使用其他符号

```
# 这是一个注释

:<<EOF
注释内容...
注释内容...
注释内容...
EOF

:<<!
注释内容...
注释内容...
注释内容...
!

```

## shell 参数传递

我们可以在执行 Shell 脚本时，向脚本传递参数，脚本内获取参数的格式为：\$n。n 代表一个数字，1 为执行脚本的第一个参数，2 为执行脚本的第二个参数，以此类推……

\$0 为 执行文件名称

```
echo "shell 参数传递"
echo "执行文件为: $0";
echo "传递的第一个参数是: $1";
echo "传递的第二个参数是: $2";
echo "传递的第三个参数是: $3";
```

脚本执行： `sh test.sh param1 param2 params3`

## shell 基本运算符

原生 bash 不支持简单的数学运算，但是可以通过其他命令来实现，例如 awk 和 expr，expr 最常用。

expr 是一款表达式计算工具，使用它能完成表达式的求值操作。

注意事项：

- 使用的是反引号 ` 而不是单引号 '
- 表达式和运算符之间要有空格，例如 2+2 是不对的，必须写成 2 + 2，这与我们熟悉的大多数编程语言不一样。
- 完整的表达式要被 `` 包含
- 乘号(\*)前边必须加反斜杠(\)才能实现乘法运算；
- 在 MAC 中 shell 的 expr 语法是：\$((表达式))，此处表达式中的 "\*" 不需要转义符号 "\" 。

```
val=`expr 2 + 2`
echo "两数之和为: $val"
```

### 关系运算符

运算符 说明 举例
-eq 检测两个数是否相等，相等返回 true。 [ $a -eq $b ] 返回 false。
-ne 检测两个数是否不相等，不相等返回 true。 [ $a -ne $b ] 返回 true。
-gt 检测左边的数是否大于右边的，如果是，则返回 true。 [ $a -gt $b ] 返回 false。
-lt 检测左边的数是否小于右边的，如果是，则返回 true。 [ $a -lt $b ] 返回 true。
-ge 检测左边的数是否大于等于右边的，如果是，则返回 true。 [ $a -ge $b ] 返回 false。
-le 检测左边的数是否小于等于右边的，如果是，则返回 true。 [ $a -le $b ] 返回 true。

```
a=10
b=20

if [ a -eq b]
then
  echo "$a -eq $b : a 等于 b"
else
  echo $a -eq $b : a 不等于 b"
fi

```

### 布尔运算符

运算符 说明 举例
! 非运算，表达式为 true 则返回 false，否则返回 true。 [ ! false ] 返回 true。
-o 或运算，有一个表达式为 true 则返回 true。 [ $a -lt 20 -o $b -gt 100 ] 返回 true。
-a 与运算，两个表达式都为 true 才返回 true。 [ $a -lt 20 -a $b -gt 100 ] 返回 false。

```
a=10
b=20

echo "$a -ge 10 -o $b -le 30"
if [ $a -ge 10 -o $b -le 30 ]
then
  echo " a 大于等于 10 或者 b 小于等于 30"
fi

if [ $a != $b -a $a -gt 5 ]
then
  echo "a 不等于 b 且 a 大于等于 5"
fi

```

### 逻辑运算符

运算符 说明 举例
&& 逻辑的 AND [[ $a -lt 100 && $b -gt 100 ]] 返回 false
|| 逻辑的 OR [[ $a -lt 100 || $b -gt 100 ]] 返回 true

### 字符串运算符

运算符 说明 举例
= 检测两个字符串是否相等，相等返回 true。 [ $a = $b ] 返回 false。
!= 检测两个字符串是否相等，不相等返回 true。 [ $a != $b ] 返回 true。
-z 检测字符串长度是否为 0，为 0 返回 true。 [ -z $a ] 返回 false。
-n 检测字符串长度是否不为 0，不为 0 返回 true。 [ -n "$a" ] 返回 true。
$	检测字符串是否为空，不为空返回 true。	[ $a ] 返回 true。

```
str="abcdef"

if [ -z str ]
then
  echo "str 长度为0"
else
  echo "str 长度不为0"

```

### 文件测试运算符

文件测试运算符用于检测 Unix 文件的各种属性。

操作符 说明 举例
-b file 检测文件是否是块设备文件，如果是，则返回 true。 [ -b $file ] 返回 false。
-c file 检测文件是否是字符设备文件，如果是，则返回 true。 [ -c $file ] 返回 false。
-d file 检测文件是否是目录，如果是，则返回 true。 [ -d $file ] 返回 false。
-f file 检测文件是否是普通文件（既不是目录，也不是设备文件），如果是，则返回 true。 [ -f $file ] 返回 true。
-g file 检测文件是否设置了 SGID 位，如果是，则返回 true。 [ -g $file ] 返回 false。
-k file 检测文件是否设置了粘着位(Sticky Bit)，如果是，则返回 true。 [ -k $file ] 返回 false。
-p file 检测文件是否是有名管道，如果是，则返回 true。 [ -p $file ] 返回 false。
-u file 检测文件是否设置了 SUID 位，如果是，则返回 true。 [ -u $file ] 返回 false。
-r file 检测文件是否可读，如果是，则返回 true。 [ -r $file ] 返回 true。
-w file 检测文件是否可写，如果是，则返回 true。 [ -w $file ] 返回 true。
-x file 检测文件是否可执行，如果是，则返回 true。 [ -x $file ] 返回 true。
-s file 检测文件是否为空（文件大小是否大于 0），不为空返回 true。 [ -s $file ] 返回 true。
-e file 检测文件（包括目录）是否存在，如果是，则返回 true。 [ -e $file ] 返回 true。

## echo 命令

Shell 的 echo 指令与 PHP 的 echo 指令类似（node 中的 console），都是用于字符串的输出

命令格式： `echo <content>` eg: `echo "this is a text"`

### -e 开始转义

```
echo -e "换行 \n  第二行"
echo "换行无效 \n  第二行"
```

### 显示命令执行结果

这里使用的是反引号 `, 而不是单引号 '。

```
echo `date`
```

## printf 命令

printf 命令模仿 C 程序库（library）里的 printf() 程序。
printf 由 POSIX 标准所定义，因此使用 printf 的脚本比使用 echo 移植性好。
printf 使用引用文本或空格分隔的参数，外面可以在 printf 中使用格式化字符串，还可以制定字符串的宽度、左右对齐方式等。默认 printf 不会像 echo 自动添加换行符，我们可以手动添加 \n。

基本语法： `printf format-string [arguments...]`
参数说明：

- format-string: 为格式控制字符串
- arguments: 为参数列表。

```
printf "%-10s %-8s %-4s\n" 姓名 性别 体重kg
printf "%-10s %-8s %-4.2f\n" 郭靖 男 66.1234
printf "%-10s %-8s %-4.2f\n" 杨过 男 48.6543
printf "%-10s %-8s %-4.2f\n" 郭芙 女 47.9876


!<<EOF
输入内容

姓名     性别   体重kg
郭靖     男      66.12
杨过     男      48.65
郭芙     女      47.99
EOF
```

> %s %c %d %f 都是格式替代符

> %-10s 指一个宽度为 10 个字符（-表示左对齐，没有则表示右对齐），任何字符都会被显示在 10 个字符宽的字符内，如果不足则自动以空格填充，超过也会将内容全部显示出来。

> %-4.2f 指格式化为小数，其中.2 指保留 2 位小数。

## test 命令

Shell 中的 test 命令用于检查某个条件是否成立，它可以进行数值、字符和文件三个方面的测试。

## 流程控制

### if else 语句

```
num1=100
num2=100

if $num1 -ge $num2
then
  echo "num1 大于等于 num2"
elif $num1 -le $num2
then
  echo "num1 小于 num2"
fi
```

### for 循环

```

for item in 1 2 3 4 5 6
do
  echo $item
done

```

### while 语句

```
int=1
while(( $int<=5 ))
do
    echo $int
    let "int++"
done
```

### until 循环

until 循环执行一系列命令直至条件为 true 时停止。
until 循环与 while 循环在处理方式上刚好相反。
一般 while 循环优于 until 循环，但在某些时候—也只是极少数情况下，until 循环更加有用。

```
a=0

until [ ! $a -lt 10 ]
do
   echo $a
   a=`expr $a + 1`
done
```

### case 语句

echo '输入 1 到 4 之间的数字:'
echo '你输入的数字为:'
read aNum
case \$aNum in 1) echo '你选择了 1'
;; 2) echo '你选择了 2'
;; 3) echo '你选择了 3'
;; 4) echo '你选择了 4'
;;
\*) echo '你没有输入 1 到 4 之间的数字'
;;
esac

### break 命令

break 命令允许跳出所有循环（终止执行后面的所有循环）。

```
while :
do
    echo -n "输入 1 到 5 之间的数字:"
    read aNum
    case $aNum in
        1|2|3|4|5) echo "你输入的数字为 $aNum!"
        ;;
        *) echo "你输入的数字不是 1 到 5 之间的! 游戏结束"
            break
        ;;
    esac
done
```

### continue

continue 命令与 break 命令类似，只有一点差别，它不会跳出所有循环，仅仅跳出当前循环。

```
while :
do
  echo -n "输入 1 到 5 之间的数字: "
  read aNum
  case $aNum in
      1|2|3|4|5) echo "你输入的数字为 $aNum!"
      ;;
      *) echo "你输入的数字不是 1 到 5 之间的!"
          continue
          echo "游戏结束"
      ;;
  esac
done
```

## Shell 函数

shell 中函数的定义格式如下：

```
[ function ] funname [()]
{
  action;
  [return int;]
}
```

注意：

- 可以带 function fun() 定义，也可以直接 fun() 定义,不带任何参数。
- 参数返回，可以显示加：return 返回，如果不加，将以最后一条命令运行结果，作为返回值。 return 后跟数值 n(0-255)
- 先定义再执行
- 函数返回值在调用该函数后通过 \$? 来获得。

```
funWithReturn(){
    echo "这个函数会对输入的两个数字进行相加运算..."
    echo "输入第一个数字: "
    # 将输入的内容赋值给aNum
    read aNum
    echo "输入第二个数字: "
    # 将输入的内容赋值给anotherNum
    read anotherNum
    echo "两个数字分别为 $aNum 和 $anotherNum !"
    return $(($aNum+$anotherNum))
}
funWithReturn
# 函数返回值在调用该函数后通过 $? 来获得。
echo "输入的两个数字之和为 $? !"

```

### 函数参数

在函数体内部，通过 $n 的形式来获取参数的值，例如，$1 表示第一个参数，$2表示第二个参数
__注意__： $10 不能获取第十个参数，获取第十个参数需要${10}。当n>=10时，需要使用${n}来获取参数。

```
funWithParam(){
    echo "第一个参数为 $1 !"
    echo "第二个参数为 $2 !"
    echo "第十个参数为 $10 !"
    echo "第十个参数为 ${10} !"
    echo "第十一个参数为 ${11} !"
    echo "参数总数有 $# 个!"
    echo "作为一个字符串输出所有参数 $* !"
}
funWithParam 1 2 3 4 5 6 7 8 9 34 73

```

几个特殊字符用来处理参数
参数处理 说明
$#	传递到脚本或函数的参数个数
$\* 以一个单字符串显示所有向脚本传递的参数

$$
$!	后台运行的最后一个进程的ID号
$@	与$*相同，但是使用时加引号，并在引号中返回每个参数。
$-	显示Shell使用的当前选项，与set命令功能相同。
$?	显示最后命令的退出状态。0表示没有错误，其他任何值表明有错误。

## Shell 输入/输出重定向

## Shell 文件包含（引入外部脚本）
Shell 也可以包含外部脚本。这样可以很方便的封装一些公用的代码作为一个独立的文件。
Shell 文件包含的语法格式如下：
```
. filename   # 注意点号(.)和文件名中间有一空格

或

source filename
```
$$
