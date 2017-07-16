#Android产生随机数

##1.产生指定位数的随机数

以产生6位数随机数为例

```int num = (int) ((Math.random() * 9 + 1) * 100000);```

产生其他位数随机数在最后面要乘的数即 *100000* 后加0即可


##2.产生指定范围的随机数

以产生[10,99]范围内的随机数为例

```
int min=10;
int max=99;
Random random = new Random();
int num = random.nextInt(max)%(max-min+1) + min;
```

end ~