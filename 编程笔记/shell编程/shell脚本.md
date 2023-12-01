
# 编程常用
```
显示连续累加数字
seq -s , 10 # 1,2,3,4,5,6,7,8,9,10
seq -s ' ' 10 # 1 2 3 4 5 6 7 8 9 10

# 变量的替换
var=hello123hello123
echo ${var/123/456}
hello456hello123
echo ${var//123/456}
hello456hello456

```

## if 判断
```
# 判断是否是空值 
if [ ! -n $para1 ]; then
  echo "IS NULL"
else
  echo "NOT NULL"
fi 
# 多个条件判断
#如果a>b且a<c 
if (( a > b )) && (( a < c )) 
if [[ $a > $b ]] && [[ $a < $c ]] 
if [ $a -gt $b -a $a -lt $c ] 

```
## hash处理
```
declare -A detail_hash
detail_hash=([AGly]="Aminoglycoside" [AGly_Flqn]="Aminoglycoside and ciprofloxacin" [Bla]="Beta-lactamase" [Fcyn]="Fosfomycin " [Fcd]="Fusidic acid" [Flq]="Fluoroquinolone" [Gly]="Glycopeptide " [MLS]="Macrolide-lincosamide-streptogramin" [Phe]="Chloramphenicol " [Rif]="Rifampicin" [Sul]="Sulfonamide " [Tet]="Tetracycline" [Tmt]="Trimethoprim" [Col]="Colistin" [Ntmdz]="Nitroimidazole" [Oxzln]="Oxazolidinone" [Tcm]="Tetracenomycin" [Mca]="Monoxycarbolic acid" [Unknown]="Unknown")
#export detail_hash;
#a="AGly"
#echo ${detail_hash[$a]}

for key in $(echo ${!detail_hash[*]}); do
   if [ $choice -eq $key ]; then
      echo ${detail_hash[$key]}
         exit
   fi
done
```
## 数组
```
my_array=(A B "C" D)
echo "第一个元素为: ${my_array[0]}"
echo "数组的所有元素为: ${my_array[*]}"
echo "数组的所有元素为: ${my_array[@]}"
# 数组长度
echo "数组元素个数为: ${#my_array[*]}"
echo "数组元素个数为: ${#my_array[@]}"
```


#  变量分割
```
a="123 456"

#方法一：通过数组的方式
declare -a arr
index=0
for i in $(echo $a | awk '{print $1,$2}')
do
    arr[$index]=$i
    let "index+=1"
done
echo ${arr[0]}
echo ${arr[1]}

# 方法二：通过eval+赋值的方式
b=""
c=""
eval $(echo $a | awk '{ printf("b=%s;c=%s",$1,$2)}')
echo $b

# 方法三
m=""
n=""
m=`echo $a | awk '{print $1}'`
n=`echo $a | awk '{print $2}'`
echo $m
echo $n
```



<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE4NTU0OTE5NjVdfQ==
-->