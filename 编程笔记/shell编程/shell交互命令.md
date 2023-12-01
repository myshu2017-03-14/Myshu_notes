[toc]
# 快捷键
```
# 设置鼠标光标快捷键
Ctrl + a/e  # 光标移动到行首/行尾
Ctrl + u   # 光标之前的全部删除
Ctrl + k   # 光标之后的全部删除

cd - # 转到最近一次的上一个目录

```

# 常用命令
```shell
# 查看文件/文件夹的真实路径
readlink -f /path/or/file  

ls -R # 列出所有文件（包括子目录）
ls -a # 列出所有文件（包括隐藏的文件，如. ..）
ls -l |grep "^-"|wc -l #统计某文件夹下文件的个数
ls -l |grep "^ｄ"|wc -l #统计某文件夹下目录的个数
ls -lR|grep "^-"|wc -l #统计文件夹下文件的个数，包括子文件夹里的

tree outdir/ # 显示文件夹文件（树状图形式）

less -SN file #按行显示文件，并显示行号
echo -e "$A\t$B" > 1.txt # 包含变量

column -tx file # 列对齐输出
nl file # 显示行号
stat file # 显示文件统计信息

#统计当前目录下所有文件夹的大小
du -ah --max-depth=1

mkdir -p # -p, --parents 可以是一个路径名称。此时若路径中的某些目录尚不存在,加上此选项后,系统将自动建立好那些尚不存在的目录,即一次可以建立多个目录;

# 删除test/文件夹下除了file1和file2之外的其他文件
# 打开extglob
shopt -s extglob
cd test/
rm -f !(file1|file2)
# 关闭extglob
shopt -u extglob
```
## tar 解压压缩
```
# tar压缩文件夹，包括文件夹链接文件的源文件
tar -zchf test.tar.gz test/  
	-z 通过gzip过滤归档文件
	-c 新建文件
	-f 存档文件在本地
	-h 获取链接文件的源文件
# tar 解压
tar -pxvzf interproscan-5.36-75.0-*-bit.tar.gz
	-p 保留权限
	-x 提取文件
	-v 查看解压进度过程
```
## 设置文件权限
```
chmod 600 file — you can read and write - good for files
chmod 700 file — you can read, write, and execute - good for scripts
chmod 644 file — you can read and write, and everyone else can only read - good for web pages
chmod 755 file — you can read, write, and execute, and everyone else can read and execute - good for programs that you want to share
```
## 查找文件 grep find
```
# 查找和替换
grep [option]
	-F  # 固定字符串进行匹配
    -e pat-list   # 指定匹配模式
    -p pat-file   # 从pat-file中读取模式进行匹配
    -i # 匹配时忽略大小写
    -l # 列出匹配模式的文件名称，不是匹配结果
    -q -s # 静默模式
    -v  # 反向匹配
    -w  #单词匹配

# 根据一个文件提取另一个文件中的内容（t中包含后面这个文件中的一部分内容）
grep -f t file.txt
# 根据file2中的id在file1中进行匹配，筛选出file1中包含file2中id的序列
grep -w -f file2.txt file1.txt > filtered.txt

# 正则匹配Tab空格
grep -P "\t" test.txt
    
#-- eg1 遍历文件夹，并对文件内容进行匹配
find . -name "*.in" | xargs grep "the"
#-- eg2 
#-- eg3 
#-- eg4 

# grep使用或匹配
grep "^less\|^mv" sample_*.sh

# grep匹配含有SOX2的行，并输出下一行
# -A 1 表示输出的行中，包含匹配行的下一行 (A: after)
 grep -A 1 'SOX2' test.fasta 

```

## 文件排序
```
sort -u file #去除重复行
sort -r file #降序输出
sort -r number.txt -o number.txt #可以输出到源文件中，源文件直接被覆盖
sort -n file #按照数值排序，默认是按照字符排序
sort -t : -k 2 #-t指定分隔符， 指定TAB分隔：sort -t $'\t' ； -k指定列
sort file | uniq -d  # 获得重复的行。(d=duplication)
sort file | uniq -c  # 获得每行重复的次数。
```

## 查看编辑文件
```shell
# 新建文件
touch file.txt

#使用cat -A 可以显示文件中所有的符号
# ^I 表示tab键
# $表示行尾
sed 's/^\(>.*\)/\1\t/' test.fasta | cat -A
>SOX2^I$
ACGAGGGACGCATCGGACGACTGCAGGACTGTC$
ACGAGGGACGCATCGGACGACTGCAGGACTGTC$
ACGAGGGACGCATCGGACGACTGCAGGAC$

# 输出文件每一行的列数
cat a.xls | awk '{print NF}' 

# shell中大小写转换（来源： http://zzuwxf.iteye.com/blog/1392421）
有两种方式：
- 用tr [optins] source replace
	-c 取source的反义
    -d 表示删除source而不是替换
    -s 浓缩source，如果出现多个则保留一个
    例如：UPPERCASE=$(echo $VARIABLE | tr '[a-z]' '[A-Z]')   (把VARIABLE的小写转换成大写)
            LOWERCASE=$(echo $VARIABLE | tr '[A-Z]' '[a-z]')   (把VARIABLE的大写转换成小写)
- 用typeset
    typeset -u VARIABLE  (把VARIABLE的小写转换成大写)
    typeset -l  VARIABLE  (把VARIABLE的大写转换成小写)
    例如：typeset -u VARIABLE
            VARIABLE="True"
            echo $VARIABLE
            输出为TRUE
# 统计命令
#通过改变$1可以指定哪一列，默认$1为第一列
列求和： cat you.txt |awk '{a+=$1}END{print a}'
    awk '{ x += $3 } END { print x }' myfile  # 第三列求和
列求平均值：cat you.txt |awk '{a+=$1}END{print a/NR}'
列求最大值：cat you.txt |awk 'BEGIN{a=0}{if ($1>a) a=$1 fi}END{print a}'  
行求和 ： awk 'BEGIN{a[$1]=0}{for(i=1; i<=NF; i++) a[$1]+=$i}END{for(j in a) print j" " a[j] > "output.dat"}' <input.dat s

#统计file2中有，file1中没有的行
grep -vFf file1 file2
# 统计某一列值重复值的个数
cut -f 3 file.out | sort | uniq -c >count.txt

# 利用perl正则替换某一行的内容
# -p 表示匹配每一行
# -e 表示后面程序写为一行
# -i 表示替换结果写回原文件
perl -p -i -e 's/^(>\S+?)\s.*/$1/g' combined_seqs.trim.fasta

# 去重复
sort -n $file | uniq  
sort -n $file | awk '{if($0!=line)print; line=$0}'  
sort -n $file | sed '$!N; /^.?\n\1$/!P; D' 

# 取交集并集
1. 取出两个文件的并集(重复的行只保留一份)
2. 取出两个文件的交集(只留下同时存在于两个文件中的文件)
3. 删除交集，留下其他的行
1. cat file1 file2 | sort | uniq > file3
2. cat file1 file2 | sort | uniq -d > file3
3. cat file1 file2 | sort | uniq -u > file3 


```

## sed替换
```
#比如从第3行到第10行
sed -n '3,10p' myfile > newfile
# 提取某个文件夹的前10个文件并cat
ls bacteria/*.fna | sed -n '1,10p' | xargs cat  >test
# 删除Love开头的行
sed -i '/^Love/d' 1.txt
sed -i '3d' 1.txt
# 删除指定行
n=1
sed -e "${n}d" -i mm10_genome.txt

sed -e 's/<[^>]*>//g' myfile.html 
#在上例中，'[^>]' 指定“非 '>'”字符，其后的 '*' 完成该表达式以表示“零或多个非 '>' 字符”。对几个 html 文件测试该命令，将它们管道输出到 "more"，然后仔细查看其结果。
# 正则替换  \(.*\)  选中，然后\1表示第一个区域的匹配内容
echo here365test | sed 's/.*ere\([0-9]*\).*/\1/g'  # 365

# sed替换指定行的内容
sed -i '1s/[#*]/fff/gp' file    #表示针对文件第1行，将其中的#号或是*号替换为fff

# 去除字符串后面的空格
n=`echo $l | cut -f 1 | sed -e 's/[ ]*$//g'`
# 去除前面的空格
sed -e 's/^[ ]*//g' 

1、在文件的首行插入指定内容：
sed -i "1i#! /bin/sh -" a  # 执行后，在a文件的第一行插入#! /bin/sh -
2、在文件的指定行（n）插入指定内容：
sed -i "niecho "haha"" a 

# 多次匹配（用分号分隔）
# 替换表头空格为下划线，并全部小写
sed -i '1s/ \+/_/g;1s/[A-Z]/\l&/g' {files}
```

## awk语法
```
# 1、对不同行处理
# 输出第一行的标签，并对剩余的行打印第一个域，第二个域和第三个域的和
awk 'NR==1{print $1,$2"+"$3}NR!=1{print $1,$2+$3}' file
# 对除第一行之外行的第二个域求和
awk 'NR!=1{sum += $2} END {print sum}' file

# 2、筛选数据
# 输出第二列小于0.05的行
awk '$2 < 0.05' file
# 如果第一第二个域都重复，那么就只保留第一次出现的行
awk '!a[$1,$2]++' file

# 3、文件拆分
# 此命令会将第一个域内容一样的行输出到以第一个域为命名的文件中
awk '{print > $1}' file

#多行变一行
# 判断第一个域是不是第一次出现，若是第一次出现，则将这一行的内容放到数组arr中，若不是第一次出现，则将第二个域的内容追加到之前出现过的那个元素后，当所有行都执行完成后输出输出arr的值。
awk '{arr[$1]=($1 in arr ? arr[$1]","$2 : $0)} END{for (k in arr) print arr[k]}' file

# 4、指定分隔符
# 指定多个分隔符，去除每行最后一个域后输出
awk -F '[;\t ]' '{NF-=1;print}' file

# 5、传参
# 上述两语句都可以实现输出文件每行的第三个域，第一个是传递一个变量的值到awk执行命令中，第二个则是将'$a'作为shell的命令，得到值3之后将其作为awk中的值。
awk -v a=3 '{print $a}' file
a=3 ; awk '{print $'$a'}' file

```
![输入图片说明](/imgs/qeoQQNDWhRNrp85b.png)

## 算术运算
```
((i=$j+$k))    等价于 i=`expr $j + $k`
((i=$j-$k))     等价于   i=`expr $j -$k`
((i=$j*$k))     等价于   i=`expr $j \*$k`
((i=$j/$k))     等价于   i=`expr $j /$k`

#通过改变$1可以指定哪一列，默认$1为第一列
列求和： cat you.txt |awk '{a+=$1}END{print a}'
列求平均值：cat you.txt |awk '{a+=$1}END{print a/NR}'
列求最大值：cat you.txt |awk 'BEGIN{a=0}{if ($1>a) a=$1 fi}END{print a}'  
#统计fasta文件有多少条序列
grep ‘>’ S_pastorianus.fasta | wc –l
#统计fastq文件中reads数目
awk '{s++}END{print s/4}' file.fastq
#统计文件夹内文件个数以及文件夹个数
#统计某文件夹下文件的个数
ls -l |grep "^-"|wc -l
ls -l |grep "^ｄ"|wc -l
# calculate mean vmem计算不同单位的一列数据的平均值的算法
grep "vmem" run.detail.15_or96 | cut -f 4 -d "," |cut -f 2 -d "=" | sed 's/M$/\/1024G/g' | sed 's/G//g' | awk '{sum+=$1} END {print "vmem average = ", sum/NR}'

```
## 集合运算
```
# 交集
sort a.txt b.txt | uniq -d

# 差集
# a.txt-b.txt:
sort a.txt b.txt b.txt | uniq -u
# b.txt-a.txt:
sort b.txt a.txt a.txt | uniq -u

# 并集
sort a.txt b.txt | uniq

# comm命令
# 要注意两个文件必须是排序和唯一(sorted and unique)的，默认输出为三列，第一列为是A-B，第二列B-A，第三列为A交B
comm a.txt b.txt  
# 仅仅排序
comm <(sort a.txt ) <(sort b.txt )  
# 排序加唯一
comm <(sort a.txt|uniq ) <(sort b.txt|uniq )  
# 只要交集
comm -12 <(sort a.txt|uniq ) <(sort b.txt|uniq )  

# grep 交集
grep -F -f a.txt b.txt  
# grep差集（顺序很重要）
grep -F -v -f b.txt a.txt | sort | uniq  
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTEzMDU5OTE1ODVdfQ==
-->