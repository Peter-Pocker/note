用缩进表示层级关系

以 key: value 指定值

字面量

值的写法

普通的值（数字，字符串，布尔）

字符串默认不需要用引号括起来。单引号会转义字符，双引号不会转义字符

字符串

对象、Map（属性和值）（键值对）

对象及其属性还是以 key: value 表示

```yaml
objectName:
	attribute1: xxx
	attribute2: yyy
```

行内写法

```yaml
objectName: {attribute1: xxx,attribute2: yyy}
```

数组（List，Set）

用“-”表示数组中的元素

```yaml
arrayname:
	- element1
	- element2
```

行内写法

```
arrayname: {}
```

