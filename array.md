# 定义数组

```
arr=("a" "b" "xxx" "1")
```

# 遍历数组

```
for item in ${arr[a]}
do
    echo "$item"
done
```

# if 语句

```
var="abcd"
if [ "$var" == "abcd" ]; then
    echo "equal"
else
    echo "not equal"
fi
```

```
check_args_valid() {
    param="invalid"
    if [ "$param" == "invalid" ]; then
        # 参数异常，返回1
        return 1
    else
        # 参数正常，返回0
        return 0
    fi
}

# 注意if条件中函数的用法，函数返回0，则条件为真
if check_args_valid; then
    echo "参数正确"
else
    echo "参数错误"
fi

```

```
param="invalid"

# if的简洁用法 [ condition ] && do_something
#等同于：
#if [ "$param" == "invalid" ]; then
#    echo "invalid"
#fi
#

[ "$param" == "invalid" ] && echo "invalid"
```
