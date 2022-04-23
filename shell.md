# shell 命令
## case 命令
  
``` shell
case "$1" in
	start)
		echo "start"
	;;
	stop)
		echo "stop"
	;;
esac
```  

## if 命令
+  整数比较

	-eq ,=
	-ne ,!=
	-gt ,>
	-ge ,>=
	-lt ,<
	-le ,<=

+ 字符串比较
	=：字符串包含的文本是否一样
	== 两个字符串是否相等
	\>:比较字母的大小，比如var1 > var2,如果var1字母大于var2则返回真
	<:和大于相反
	!= 两个字符串不相等
	-z 空字符串
	-n 非空字符串
+ 文件判断
	[ -d FILE ] ：如果 FILE 存在且是一个目录则为真。
	[ -e FILE ] ：如果 FILE 存在则为真。
	[ -f FILE ] ：如果 FILE 存在且是一个普通文件则为真。

``` shell
if [ $a -eq $b ];then
    echo "a=b"
elif [ $a -gt $b ];then
    echo "a < b"
else
    echo "a > b"
fi
+ test 命令
```
## while	
	while CONDITION；do
		statement
		statement
		<改变循环条件真假的语句>
		<if 条件>;then
			break;#退出循环
		fi
	done

	let语句是整形的运算

``` shell
sum=0
i=0
while [ $i -le 100 ];do
    let sum=$sum+$i
    let i=$i+1
	if [ $sum -eq 4656 ];then
		break;
	fi
done
echo "sum=$sum"
```
## set
	set -x 显示的打印执行过程,单独的set将会打印所有的变量值
``` shell
set -x
set
```
## declare
	declare用于声明shell变量，在第一种语法中可用来声明变量并设置变量的属性([rix]即为变量的属性），在第二种语法中可用来显示shell函数。若不加上任何参数，则会显示全部的shell变量与函数(与执行set指令的效果相同)

 +/- 　"-"可用来指定变量的属性，"+"则是取消变量所设的属性。
 -f 　仅显示函数。
 r 　将变量设置为只读。
 x 　指定的变量会成为环境变量，可供shell以外的程序来使用。
 i 　[设置值]可以是数值，字符串或运算式。

+ 声明整数型
``` shell
	declare -i ab // 声明整数型变量
	ab=56
	echo $ab

	declare +i ab // +i 取消变量变量属性,才能进一步的赋值字符串

	ab="we"

	echo $ab
```
+ 声明数组
	变量名称=(数组元素1 数组元素2 ....),例如strarray=("1" "2" "3")
``` shell
	declare -a cd=("a" "b" "c")

	echo ${cd[0]}
	echo ${cd[@]} #@ *返回数组所有元素
	echo ${#cd[*]} # #数组[*] 返回数组所有长度
	echo ${#cd[@]}

	# foreach 方式访问
	for i in ${cd[*]}
		do
			echo ${i}
		done

	# for循环访问
	for((i=0 ; i < ${#cd[*]} ; i++))
		do
			echo ${cd[i]}
		done

	# 关于数组带不带引号问题
	# 1、${cd[*]} 按照空格单个元素分开
	# 2、"${cd[*]}" 将会把整个数组当成字符串,所有都是按照不带引号操作


	declare -a err_packages #声明数组
	#数组赋值
	declare -a arr=("gcc-5" "g++-5" "libpng-dev" "zlib1g-dev" "libtiff-dev" "libsdl2-dev" "libsdl2-image-dev" \
	"graphviz" "graphviz-dev" "build-essential" "libxmu-dev" "libxi-dev" "libgl-dev" "libosmesa-dev" "python3" "python3-pip" "curl"    \
	"libz1:i386" "libc6-dev-i386" "libc6:i386" "libstdc++6:i386" "g++-multilib" "git" "diffstat" "texinfo"\
	"gawk" "chrpath" "libfreetype6-dev" "mono-runtime" "flex" "libssl-dev" "u-boot-tools" "libdevil-dev"  \
	"bison" "python3-pyelftools" "python3-dev" "libx11-dev")

	# 遍历数组
	if ! sudo apt-get install -y "${arr[@]}"; then
	for i in "${arr[@]}"
	do
		if ! sudo apt-get install -y "$i"; then
		err_packages+=("$i")
		fi
	done
	fi

```
## 函数
	函数定义：函数名(){ }
+ 函数定义和入参
``` shell

	fun(){ #函数定义
		echo $@#$@函数所有入参
		echo $##函数入参个数
		for i in $@
			do
				echo ${i}
			done
	}

	fun 1 2 3 4 5 "fun"

# 数组作为入参
array=("A" "B" "C")
fun(){
    
    local arr=($@)
    for i in ${arr[*]}
        do
            echo "$i"
        done 
}
fun ${array[*]}
```
+ 函数返回值
函数返回值只能是通过全局变量或者echo等方式返回
``` shell
	fun1(){
		echo $1
	}

	ret=$(fun1 10)
	echo "ret = ${ret}"

	declare -i sum=0

	fun2(){
		for((i = 0 ; i < 100 ; i++))
			do
				let sum+=${i}
			done
		return $?
	}

	fun2
	echo "sum = ${sum}"

	# 数组作为返回值

	fun3(){
		
		local array=("A" "B" "C")

		echo ${array[@]}
	}

	array_fun_ret=$(fun3)

	echo "array_fun_ret=${array_fun_ret[@]}"


```
