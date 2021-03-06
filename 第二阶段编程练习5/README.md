# 第二阶段编程练习5

本次作业着重练习了字符串的各种操作，在了解原理的基础上灵活使用cstring库函数就可以比较容易地求解。着重介绍几个比较重要的字符串操作和库函数。

1. 字符串一定以'\0'结尾，利用这个性质，可以求取字符串长度。
2. 字符串其实也是字符指针，因此可以利用指针特性来直接操作。
3. strlen(s)，返回字符串s的长度。
4. strcmp(s1, s2)，若字符串s1和s2相等，则返回0，否则返回非0数。
5. strcpy(t, s)，将字符串s拷贝到字符串t中。

## 判断字符串是否回文

回文串是正读和倒读都完全相同的字符串。因此，我们首先构造输入的字符串的倒串，然后再比较正串与倒串是否相同即可。

构造倒串本来可以使用strrev函数，但是编程网格不支持使用该函数，所以还需要手动倒置输入的字符串。之后可以手工比较字符串各位或者直接使用strcmp函数。

## 字符排序

该题目需要首先找到左半串和右半串以及中点，之后再进行排序和交换位置。其中排序算法已经在之前的作业中有所接触，因此，该题的难点是在找分界点。

分类讨论字符串长度的奇偶性。给定字符串str，规定左半串的范围为[bol, eol)，右半串的范围为[bor, eor)。

当字符串长度为偶数时，此时不需要定位中点，只要找到左右两个半串即可。此时可以发现bol，eol，bor和eor满足下表。

| 0 | 1 | 2 | 3 | 4 | len=4 |
|-|-|-|-|-|-|
| 'p' | 'e' | 'a'| 'r' | '\0' |
| bol | | eol ||| bol=0, eol=len/2 |
||| bor | | eor | bor=len/2, eor=len |

当字符串长度为偶数时，需要找到左右两个半串，以及中点不做操作的字符位置。此时可以发现bol，eol，bor和eor满足下表，并且中点就是eol。

| 0 | 1 | 2 | 3 | 4 | 5 | len=5 |
|-|-|-|-|-|-|-|
| 'a' | 'p' | 'p'| 'l' | 'e' | '\0' |
| bol | | eol |||| bol=0, eol=len/2 |
|||| bor | | eor | bor=(len+1)/2, eor=len |

综上可以发现，不论字符串长度如何，bol=0，eol=len/2，eor=len都是确定的；仔细考虑可以发现bor=(len+1)/2在int类型下是都成立的。

之后的步骤参考提供的代码即可。

## 单词倒排

可以利用字符串以'\0'结尾的特性来巧妙地完成该题目。当我们知道整个字符串的起始位置后，可以将中间所有空格替换为'\0'，之后从右向左输出，就能直接输出所有单词。

## 班级学生成绩统分

这是一道练习结构体和排序算法的题目。结构体的部分，掌握基本语法就可以完成；难点在于排序。

直接使用冒泡排序来对所有学生的成绩排序，是会直接超时的。因为冒泡排序的复杂度是O(n^2)，在100k的数据量下，计算量达到10G级别，远超计算机1s的计算能力（大约在G级）。因此不能直接冒泡排序，我们可以只进行部分排序，将前三大的数找到即可；或者使用更快的排序算法，例如快速排序的复杂度是O(nlogn)，计算量大约为30M，是可以支持的。

## 统计单词

题目中的单词可能包括a-z小写字母，A-Z大写字母，0-9数字，以及单引号'，我们将判断一个字符s是否在单词中的函数写作is_letter(s)。

类似于单词倒排，我们首先将所有不是单词中字符的分隔字符用'\0'替换，方便之后的操作和判断。使用二维char数组word来记录出现过的单词，使用int数组统计word中对应单词出现的次数。当我们从字符串中发现一个单词时，使用如下方法来进行统计。

```
在word中存放了n个单词时，给定单词s
1. 从i=0到n-1检查word[i]与s是否相等
    1.1 若word[i]与s相等，则cnt[i]加一计数
    1.2 否则，继续1的检查
2. word中n个单词均不与s相等
    2.1 将s存放于word[n]
    2.2 cnt[n] = 1
    2.3 n加一，word中多了一个单词
```

最后，仅需要知道如何从处理过的字符串中找到各个单词即可完成题目。算法如下，flag初始为true。

```
1. 从i=0到len-1检查str[i]是否是单词中的字符
2. 若is_letter(str[i])为true，说明str[i]是单词中的字符
    2.1 若此时flag为true，则说明str[i]是一个单词的开始，地址str+i起到下一个'\0'中间的字符串即为找到的单词
        2.1.1 将该单词传入前文中的统计算法
        2.1.2 设置flag为false
    2.2 若此时flag为false，则说明str[i]是一个单词中间的字符，不做操作
3. 若若is_letter(str[i])为false，说明str[i]是'\0'，即为分隔的字符
    3.1 设置flag为true
```

将以上部分组合在一起即可。