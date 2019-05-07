
# 位运算

## 位运算实现数的相加

+ ^ 异或运算，只要有一个位上是1就为1，除开两个都是1时
+ & 与运算，两个位上都是1结果就为1，其余为0
  0111 & 0101 = 0101 十进制 = 5
+ << 左移
  0101 << 1 = 01010 十进制 = 10

```javascript
const bin = (a,b) => {
  let add = a ^ b;
  let carray = (a&b)<<1;
  for(;carray!=0;){
    add = a ^ b;
    carray = (a&b)<<1;
    a = add;
    b = carray;
  }
  return add;
}
```
运算过程如下：
```
// ^ 异或运算，只要二进制位上有 1 结果就为 1,并且两个位不同时为1
// 比如 3 => 011, 2 => 010, 那么 011 ^ 010 = 001 = 1
// & 与运算
// 两个位上面都是 1 结果就是 1
// 比如 3 => 011, 2 => 010, 那么 011 & 010 = 010 = 2 
// << 左移，将整个二进制位向左移动
// 比如 2 => 010, 那么 010 << 1(左移一位) = 100 = 4

可以从中看出，二进制的运算是冯2进一，当 3+2 大于了2时, 向左进一位,然后保留最小位的值
也就是说如果当两个数相 与(&) 后, 如果结果是大于最小位的数时，则会向左进一位
而左移又恰巧是将位左移一位得到左边位的值
1<<1 = 2
2<<1 = 4
3<<1 = 6
所以，位运算加法思路是：
1.利用异或运算得到相加两个数的个数
2.利用与运算得到两个数相加的进制数
3.利用左移拿到进制数的值

如下：
a=5
b=7

5^7 = 0101 ^ 0111 = 0010 = 2 // 两个数相加的个位数为 2
5&7 = 0101 & 0111 = 0101 = 5  
5<<1 = 0101 << 1 = 01010 = 10 // 表示进制数为 10

依次一直循环，直到两个数相加没有进制数
2^10 = 0010 ^ 1010 = 1000 = 8
2&10 = 0010 & 1010 = 0010 = 2
2<<1 = 0010 << 1 = 100 = 4

依次一直循环，直到两个数相加没有进制数
4^8 = 0100 ^ 1000 = 1100 = 12
4&8 = 0100 & 1000 = 1100 = 0
两个数的进制数已经为0，循环结束

最后结果就为 12
```

## 减法

减法操作可以用加法操作来实现。例如 a - b = a + (-b) = a + (~b + 1)
+ ~ 取反运算，所有位变成相反值，1变0，0变1
*是以标准8位二进制数做取反*
0000 0101  = 5 的二进制码
1111 1010  = 先取补码
1111 0101  = 补码的反码（符号位不变）
1111 0110  = 反码+1
1111 0110  = 这就是5的按位取反结果 -6

# 圆的面积 

```java
public double getArea(int r){
  double result = 0;
  // 需要将接受的半径转换成 double
  result = (int)r * (int)r * Math.PI;
  return result;
}
```

```javascript
const getArea = r =>　{
  return r * r * Math.PI
}
```

```python
def getArea(r):
  return r * r * Math.PI
```

# 找出两个数之间满足条件的数，并取合

```javascript
const getSum = (A, B) => {
  let result = [0]
  if(A > B){
    //[A,B] = [B,A]
    return 0
  }
  for(let i = A; i <= B; ++i){
    if (i%3 == 0) {
      result.push(i)
    }
  }
  return result.reduce((prev, cur) => prev + cur)
}

```

# 字符串匹配

时间复杂度:O(nm)

```javascript
const subStr = (master='ababababaadb', parrten='abaca') => {
  let target = -1
  if (master.length <= parrten ) {
    return target
  }
  let i,j;
  for( i = 0; i < master.length; ++i ){
    for( j = 0; j < parrten.length; ++j ){
      if ( master[i + j] != parrten[j]){
        break;
      }
    }
    if (j >= parrten.length) {
      target = i
      break;
    }
  }
  return target
}
```

# KMP
利用子串临时数组
时间复杂度:O(n+m)

```javascript
const computeTemporaryArray = pattern => {
  let lps = []
  let index = 0;
  for(let i = 1; i < pattern.length;){
    if(pattern[i] == pattern[index]){
      lps[i] = index + 1;
      index++;
      i++;
    }else{
      if(index != 0){
        index = lps[index-1];
      }else{
        lps[i] = 0;
        i++;
      }
    }
  }
  return lps;
}

const KMP = ( text, pattern ) => {        
  let lps = computeTemporaryArray(pattern);      
  let i = 0;
  let j = 0;
  let target = -1
  while( i < text.length && j < pattern.length ){
    if( text[ i ] == pattern[ j ] ){
      i++;
      j++;
    } else {
      if( j != 0 ){
        j = lps[ j-1 ];
      }else{
        i++;
      }
    }
    if( j == pattern.length ){
      // 此时子串已经在主串中匹配完毕
      // i 和 j 都已经自增一次，所以 i - j 就是匹配第一次的长度
      target = i - j
      break
    }
  }
  return target
}
```