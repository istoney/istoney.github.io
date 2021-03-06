---
layout: post
title: Algorithm Problems Summary
category: Job Hunting
tags: [job, algorithm, leetcode]
---
{% include JB/setup %}

> - 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24, 26, 27, 32, 39,
> - 42, 43, 44, 65, 66, 67, 77, 84, 80, 91,
> - 121, 122, 123, 128, 234, 273, 283, 301, 371,

## 1. 线性表

### 1.1 数组

#### 1.1.1 Unclassified

[Source Code](/job%20hunting/2016/06/21/leetcode-problems-array#unclassified)

[27 - Remove Elements](https://leetcode.com/problems/remove-element/) 给定整数数组和整数值val，从数组中原地移除等于val的元素。

- index标记元素重排后的下一位置；遍历数组，若第i个元素与val不同，将其放置在index上，并将index后移。

[283 - Move Zeros](https://leetcode.com/problems/move-zeroes/) 将给定数组中的0移动到数组尾部。

- 同#27，先移除所有0元素，然后在数组尾部空余位置补0.

[26 - Remove Duplicates from Sorted Array](https://leetcode.com/problems/remove-duplicates-from-sorted-array/) 原地移除排序数组里的重复数字。

- index标记移除重复元素重排后的下一位置；遍历数组，若第i个元素与前一元素不同，将其放置在index上，并将index后移。

[80 - Remove Duplicates from Sorted Array II](https://leetcode.com/problems/remove-duplicates-from-sorted-array-ii/) 每个元素最多可以出现两次。

- 增加status记录当前重复次数，nums[i]与前一元素不同时status为1，元素相同且status为1时，将status赋值为2.

[128 - Longest Consecutive Sequence](https://leetcode.com/problems/longest-consecutive-sequence/) 在一个未排序的整数数组中，求最长连续数字序列的长度。要求时间复杂度为\\( O(n) \\).

- 查找连续数字序列，又要求时间复杂度为\\( O(n) \\)，因此采用哈希，哈希的key是数字本身，value是该数字所在的区间的首尾数字；
- 计算中要保证一个区间的首尾数字的value值是正确的，区间内部数字的value值不需要更新；
- 遍历数组，若nums[i]在哈希中存在，跳过；
- 若不存在，检查nums[i]-1和nums[i]+1：
    + 若都存在，将两个区间合并，并更新新区间的首尾，以及nums[i]的value；
    + 若只存在一个，扩展该区间，并更新区间首尾的value；
    + 若都不存在，创建一个只包含nums[i]的区间，更新nums[i]的value。

#### 1.1.2 求和

[1 - Two Sum](https://leetcode.com/problems/two-sum/) 给定整数数组，返回和为目标值的两个数的下标，结果唯一.

- 排序后双指针夹逼，因为结果为下标，排序时需要将下标跟随值异同排序，复杂度\\( O(nlogn) \\)；
- Hash表记录每个数及下标，遍历查找，复杂度\\( O(n) \\).

[15 - 3Sum](https://leetcode.com/problems/3sum/) 给定整数数组，返回所有和为0的三元组，不得重复.

- 对数组排序，然后从头遍历数组选取第一个数，再从第一个数之后的范围内选取另外两个数；**第一个数<=0,可进行剪枝**，且遇到相同数只计算一次。复杂度\\( O(n^2) \\).

[16 - 3Sum Closest](https://leetcode.com/problems/3sum-closest/) 给定一个数组和一个目标值target，找出和与target最接近的三个数，将和返回。

- 同3Sum，先排序，遍历选取第一个数，然后双指针夹逼计算twoSum.

[18 - 4Sum](https://leetcode.com/problems/4sum/) 给定一个整数数组和目标值target，找出所有和为target的四元组。

- 类似3Sum，排序后遍历选取第一个数，然后从第一个数后再遍历选取第二个数，然后再第二个数之后的范围内就算twoSum. 计算时注意剪枝, 复杂度\\( O(n^3) \\).
- 首先计算所有两个数的和，存入Hash表，替换twoSum计算, 复杂度\\( O(n^2) \\).

**类型总结**

- 求k个数的和等于target，首先对数组排序；
- 若k为2，则采用双指针夹逼，若k大于2，则遍历元素，将其转化为k-1；
- 注意剪枝，例如遍历时，k个连续元素和大于target，则停止遍历...
- 可采用Hash表缓存两个数的和，替换twoSum计算；

#### 1.1.3 容积 & 面积

[Source Code](/job%20hunting/2016/06/21/leetcode-problems-array#section-1)

[11 - Container With Most Water](https://leetcode.com/problems/container-with-most-water/) X轴上有n条竖直线段，每段高度为\\( a_i \\)，从中选取两条使其组成的容器装下的水最多。

- 双指针。指针i，j分别指向数组的首尾，求此时面积并记录；
- 判断i，j处线段的高度，移动高度较小的一个，i向后移动，j向前移动，直到i，j相遇。
- 证明：当height[j]<height[i]时，height[j]限制了area(i,j)的高度，则area(i+1,]，area[i+2]...都将小于area(i,j)，所以都不需要计算，将j移向下一位置即可。

[42 - Trapping Rain Water](https://leetcode.com/problems/trapping-rain-water/) 给定N个非负整数代表X上N个高度为\\( a_i \\)，宽度为1的实体矩形，求下雨后矩形间可以存下多少水。

- 每个位置的宽度固定为1，其积水高度在于其左右两侧两个最高矩形的最小高度；
- 正序遍历，记录每个位置上其左侧最高高度；然后倒序遍历，记录每个位置上其右侧最高高度。
- 每个位置上积水高度为min(maxHeightLeft, maxHeightRight) - height[i]，若该值为负，表示积水为0.

[84 - Largest Rectangle in Histogram](https://leetcode.com/problems/largest-rectangle-in-histogram/) 给定N个整数代表X轴上N个高度为\\( a_i \\)的柱状图，在改图中求面积最大的矩形。

- 在位置i上，若以i的高度heights[i]构建矩形，该矩形的宽度取决于i的左侧第一个低于i的位置，以及i右侧第一个低于i的位置；
- 正向遍历数组，对每个位置i计算其左侧第一个低于其高度的位置lowHeightLeft：从j=i-1开始，若j低于i则结果为i，若j高于i，则令j=lowHeightLeft[j]，继续上述过程；0位置为-1；
- 倒序计算每个位置右侧第一个低于其高度的位置lowHeightRight，过程类似；末尾位置为heights.size()；
- 遍历，计算位置i上，以heights[i]为高度的矩形的面积：rect = heights[i] * (lowHeightRight[i] - lowHeightLeft[i] - 1)；

**类型总结**

- 求容积或面积类的问题中，关键在于查找高度变化的位置。

#### 1.1.4 Stock

[Source Code](/job%20hunting/2016/06/21/leetcode-problems-array#stock)

[121 - Best Time to Buy and Sell Stock](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/) 给定N个整数代表股票的价格，只允许一次买卖，先买后买，求最大利润。

- 先买后卖，所以在i时间点，可取的最大利润是以min(prices[0]...prices[i-1])买入，以prices[i]卖出；
- 遍历数组，记录到该位置之前的最小值，并计算此时卖出的利润，与最大利润比较，记录最大利润。

[122 - Best Time to Buy and Sell Stock II](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-ii/) 可以买卖多次，但是在买到的股票卖出之前不可以再次买入。

- 多次买卖获得最大利润的方式，是抓住所有上升段，因此在每个上升段的最低点买入，最高点卖出；

[123 - Best Time to Buy and Sell Stock](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iii/) 最多可以进行两次完整交易，求最大利润。

- 因为两次交易不可以重叠，必须第一次完成后再开始第二次，所以必定存在位置k，在[0, k]的一次最大利润，加上(k, N-1]的一次最大利润，组成两次交易最大利润；
- 正向遍历中记录minPrice可以计算从0开始到当前的最大利润，则倒序遍历记录maxPrice就可以计算从末尾开始到当前的最大利润；
- 计算每个位置i上的[0, i]最大利润firstMaxProfit，以及[i, N-1]最大利润secondMaxProfit，并遍历数组计算firstMaxProfit[i]与secondMaxProfit[i+1]的最大和；
- secondMaxProfit[0]、firstMaxProfit[N-1]是两个最大的一次交易利润。

---

### 1.2 链表

[Source Code](/job%20hunting/2016/06/26/leetcode-problems-linked-list)

[19 - Remove Nth Node from End of List](https://leetcode.com/problems/remove-nth-node-from-end-of-list/) 通过一次遍历删除单链表中倒数第n的节点。

- 先后指针，第一个指针先走n步后第二个指针出发，两个指针同步后移，直到第一个指针达到尾部，此时第二个指针的下一个为要删除的节点；
- 在链表头部建一个空节点作为头节点，可以有效避免头节点被删除的情况。

[234 - Palindrome Linked List](https://leetcode.com/problems/palindrome-linked-list/) 判断一个单链表是否是回文的，O(n)时间复杂度和O(1)空间复杂度。

- 先找到链表的中点，然后把后半部分翻转，然后判断两个半链表是否相同。

[21 - Merge Two Sorted Lists](https://leetcode.com/problems/merge-two-sorted-lists/) 合并两个已排序链表。

- 构造一个空节点作为头，有序的将两个链表的节点追加在链表尾部，直到一个链表为空后，将另一个链表的剩余节点全部追加在尾部，返回头节点的next。

[23 - Merge k Sorted Lists](https://leetcode.com/problems/merge-k-sorted-lists/) 合并k个已排序链表。

- 使用**优先队列**，将k的链表的头节点放入优先队列，按照节点中Value的大小，对其进行排序。每次从队列中取出一个节点，并将其下一节点放入队列，直至队列为空。

[24 - Swap Nodes in Pairs](https://leetcode.com/problems/swap-nodes-in-pairs/) 给定单链表，交换每两个节点的先后顺序。

- 构造一个空节点作为头，向后移动两个节点并交换，直到遇到NULL，最后返回空节点的next.

![Phone Keyboard](/assets/post_img/jobhunting/leetcode24-swap-nodes.svg.png)

[25 - Reverse Nodes in k-Group](https://leetcode.com/problems/reverse-nodes-in-k-group/) 给定单链表和整数k, 将该单链表按照k长度为一组进行翻转，当剩余长度不足k时，保持该部分的原顺序。

- 从当前结点前进k步，查看剩余长度是否大于等于k；
    + 不满足时，到达尾部，结束程序；
    + 满足，k步后达到的节点为新的head节点，然后将k个节点倒序插入在新链表的尾部

---

## 2. 字符串

### 2.1 Unclassified

[6 - ZigZag Conversion](https://leetcode.com/problems/zigzag-conversion/) 按照给定行数，如下图所示Z形打印字符串，例如字符串`"PAYPALISHIRING"`的打印结果为`"PAHNAPLSIIGYIR"`.

```
P   A   H   N
A P L S I I G
Y   I   R
```

- 每行的字符按照步进间隔出现，在一个周期中，第一行和最后一行出现一个字符，其余行出现两个字符；
- 每个Z周期长度为2R-2，因此第一行和最后一行的步进为2R-2；
- 第i（从0开始）行的步进为2(R-1)-2, 第二个字符的步进为2i.

[14 - Longest Common Prefix](https://leetcode.com/problems/longest-common-prefix/) 给定一个字符串数组，求数组中所有字符串的最长公共前缀。

- 以第一个字符串作为公共前缀，遍历剩余字符串计算每个与公共前缀的公共长度，将字符串resize，继续遍历。

### 2.2 回文

[5 - Longest Palindromic Substring](https://leetcode.com/problems/longest-palindromic-substring/) 给定字符串S，求其最长回文字串，S长度最大为1000，最长字串唯一。

- 字符中间插入额外的'#'，使计算简单;
- 计算以每个字符为中心的回文串半径长度，从中选取最长；
- 计算中，记录当前最靠后的回文串尾部位置max，及其中心位置，若i小于max，则p[i] = min(p[2*max-i], max-i)；否则为1；

### 2.3 括号匹配

[20 - Valid Parentheses](https://leetcode.com/problems/valid-parentheses/) 字符串中包含'(', ')', '[', ']', '{', '}'，判断该字符串中括号是否有效匹配。

- 若字符串中只存在一种括号，则对括号的左、右字符分别计数即可；
- 该题目中存在三种括号类型，因此简单计数无法满足，所有采用**栈**进行模拟匹配。

[22 - Generate Parentheses](https://leetcode.com/problems/generate-parentheses/) 给出整数n，生成n对括号的合法组合。

- n对括号组成的字符串长度为2n，这2n个位置上，每一个位置可以是'('或')'；
- 出现')'需要满足的条件为：到当前位置'('的数量 >= ')'的数量；

[32 - Longest Valid Parentheses](https://leetcode.com/problems/longest-valid-parentheses/) 给定由'('和')'组成的字符串，求该字符串中左右括号有效匹配的最长子串。

- 计数法：单一一种括号，从0到i位置有效的规则是：
    + 0到i-1有效，且当前位置为'(';
    + 0到i-1有效，当前位置为')'，且0到i-1中'('的数量大于')'；
- 以start标记开始位置，按照上述规则查找最长有效字符串；
- 但该规则找到的括号会存在多余的'('，因此需要对该字串按照上述规则逆向处理，去除多余'('.

**类型总结**

- 计数匹配准则：
    + 只包含单一括号类型的字符串中，若0 ~ i-1有效，则0 ~ i有效的准则是左括号的数量大于等于右括号的数量（包含i）；
    + 包含多种类型的括号时，上述准则则变成**必要条件**，因此除了数量匹配还要满足不同类型的顺序，若需要记录这些顺序，则需要使用栈；

---

## 3. 数字的表示与计算

[Source Code](/job%20hunting/2016/06/23/leetcode-problems-numberic)

### 3.1 整数表示与转换

[66 - Plus One](https://leetcode.com/problems/plus-one/) 给定一个用digits数组表示的整数，高位数字在数组投头部，求该整数加一的结果。

- 要加的1可以看作最低位的进位，记录进位循环计算，直到没有进位位置；加到最高位时若存在进位，需要在数组头部insert。

[67 - Add Binary](https://leetcode.com/problems/add-binary/) 给定两个二进制字符串，计算其二进制和，仍以字符串形式返回。

- 遍历字符串相加，用一个变量记录进位；注意字符串顺序与数字顺序相反。

[2 - Add Two Numbers](https://leetcode.com/problems/add-two-numbers/) 给定两个用单链表表示的非负整数，每个节点上为1个digit，倒序存储，求这两个这个整数的和。

- 同步遍历链表相加，用一个变量记录进位，复杂度\\( O(m+n) \\).

[43 - Multiply Strings](https://leetcode.com/problems/multiply-strings/) 给定用字符串表示的两个整数，返回字符串表示的两数乘积；两数非负，可能会很大；禁止使用转换函数和内置BigInteger。

- 双重循环计算乘法，注意进位；复杂度\\( O(m*n) \\)。

[8 - String to Integer (atoi)](https://leetcode.com/problems/string-to-integer-atoi/) 实现atoi，将字符串转换成整数。

- 首先去除字符串开头的空格，然后判断其正负符号；
- 以long存储计算结果，逐字符计算整数值；
- 加上正负符号，判断结果是否超出int范围，输出对应int最大或最小值。

[65 - Valid Number](https://leetcode.com/problems/valid-number/) 给定一个字符串，判断其是否为有效数字。

- 允许开头和结尾空格，空格后可以有正负号，数字部分有如下几种模式：
- 整数：1，0，123
- 浮点数：1.5，0.12，.234, 0.0
- 科学计数法：2e-10，3.14e+9，.3e10，5.e-2
- 即，+/-若干位digits(可在任意位置包含.)[e/E[+/-]若干位digits]

[12 - Integer to Roman](https://leetcode.com/problems/integer-to-roman/) 将1~3999之间的整数转换为罗马数字。

- 罗马数字规则：
    + 字符：I  V   X   L   C   D   M
    + 数字：1  5   10  50  100 500 1000
    + 相同的数字连写表示这些数字相加，连写不超过三次；
    + 小的数字在大的数字的右边表示这些数字相加，小的数字（限于I、X、C）在大的数字的左边表示大数减小数；
- 构造每位数字的Hash表：

```c++
char[][] table = {
    {"0","I","II","III","IV","V","VI","VII","VIII","IX"},
    {"0","X","XX","XXX","XL","L","LX","LXX","LXXX","XC"},
    {"0","C","CC","CCC","CD","D","DC","DCC","DCCC","CM"},
    {"0","M","MM","MMM"}};
```

[13 - Roman to Integer](https://leetcode.com/problems/roman-to-integer/) 将罗马数字转换成数字。

- 对于小数字出现在大数字前面的情况要特殊判断；
- 初次之外，对于每个字符根据其值将结果增大。

[273 - Integer to English Words](https://leetcode.com/problems/integer-to-english-words/) 将整数转换为英文。

- 递归对每三位整数处理，每次递归在后面添加对应“Thousand”，“Million”，“Billion”。详见代码。

### 3.2 整数计算

[7 - Reverse Integer](https://leetcode.com/problems/reverse-integer/) 对整数进行数字翻转. Example1: x = 123, return 321; Example2: x = -123, return -321

- 首先判断整数的正负类型，然后逐步取余计算翻转结果，翻转结果保存在long中，最后判断结果是否超出int范围，若是返回0.

[9 - Palindrome Number](https://leetcode.com/problems/palindrome-number/) 判断一个整数是否是回文数，不要使用额外空间。

- 负数不是回文数，首先判断该整数正负；
- 每次从整数末尾去掉一个数字，用其构建新数字，直到两个数字相等或差10倍；
- 判断两数相等或除10后相等，注意末尾有0的情况。

### 3.3 二进制计算

[371 - Sum of Two Integer](https://leetcode.com/problems/sum-of-two-integers/) 不使用`+`和`-`计算符，计算两个整数的和。

1. 计算亦或作为和，两数与作为进位，进位左移一位，递归计算，直到没有进位。

---

## 4. 栈和队列

### 4.1 Stack

---

## 5. 二叉树

---

## 6. 查找

[4 - Median of Two Sorted Arrays](https://leetcode.com/problems/median-of-two-sorted-arrays/) 给定两个排序数组，求其所有数的中位数，要求算法复杂度为\\( O(log(m+n)) \\)。

1. 首先分别查找两个数组的中位数，对其进行比较，若相等返回，若一大一小，选取两个数组长度小的一个的一半作为裁剪长度，从中位数小的数组前部减去该长度，从中位数大的数组的后部减去该长度；递归。
2. 查找第k大的数：若两个数组长度均大于k/2，从两个数组分别去其第k/2大的数，若相等，返回该值；不相等，则删除值较小的所在数组的前k/2个数。若其中一个数组长度小于k/2（则另一个数组长度应大于k/2），此时去数组1的全部长度len，取数组2的k-len长度的数进行比较。递归。

---

## 7. DP

[Source code](/job%20hunting/2016/06/22/leetcode-problems-DP)

[10 - Regular Expression Matching](https://leetcode.com/problems/regular-expression-matching/) 实现支持'.'和'\*'的正则匹配。

- DP，\\( O(n^2) \\)。设模式字符串为p，被匹配字符串为s. dp[i][j]表示p[0...i-1]与s[0...j-1]是否匹配；
    + 初始化，dp[0][0]为true，第一行dp[0][j]为false，第一列dp[i][0]检查p开头为"a\*"或"a\*b\*"等；
    + 若p[i-1]=='\*'，dp[i][j] = dp[i-2][j] \|\| ((p[i-2]==s[j-1] \|\| p[i-2]=='.') && dp[i][j-1]);
    + 否则，dp[i][j] = dp[i-1][j-1] && (p[i-1] == s[j-1] \|\| p[i-1] == '.');
- 回溯，\\( O(!n) \\).
    + 双指针i，j指向s与p的当前字符；
    + 首先判断p的下一字符是否为*，若不是，判断s[i]与p[j]是否相等或p[j]等于'.'，递归；
    + 若是，遍历*匹配0, 1...个字符的情况，每种情况下递归。

[44 - Wildcard Matching](https://leetcode.com/problems/wildcard-matching/) 实现支持'?'和'\*'的正则匹配。此处'?'表示匹配任意字符，'\*'表示匹配任意字符序列，包括空。

Note：本题中?和*的匹配规则与上一题不同。

- DP。
    + 当p[i]为*时，dp[I][j] = dp[I][j-1] \|\| dp[I_1][j] \|\| dp[I_1][j-1]；
    + 否则判断p[i]与s[j]是否相等，或p[i]为?；
    + 状态压缩，DP数组只用两行即可。
- 回溯，见代码。

[91 - Decode Ways](https://leetcode.com/problems/decode-ways/) 把A-Z编码成1, 2, ..., 26，给定一数字字符串，求其有多少种编码方式。例如，“12”可以作为“AB”的编码，也可以作为“L”的编码，因此其结果为2.

- DP，类似于Fibonacci数列，dp[i] = dp[i-1] + dp[i-2];
    + 需要注意0数字，只有当0出现在1或2的后面才是有效的，否则就是非法的。

---

## 8. DFS & BFS & 枚举

[Source Code](/job%20hunting/2016/06/25/leetcode-problems-brute-force)

### 暴力枚举

[17 - Letter Combinations of a Phone Number](https://leetcode.com/problems/letter-combinations-of-a-phone-number/) 给定一个数字字符串，按照手机键盘的数字与字母对应关系，求该数字字符串可能代表的字母组合。

![Phone Keyboard](/assets/post_img/jobhunting/leetcode17-Telephone-keypad2.svg.png)

- 递归穷举：先生成后面n-1个数字组成的字母组合，然后再在这些字符串的头部分别加上第一个数字对应的字母；
- 0对应空格，1不对应字母，因此当存在0或1时，结果为空。



### DFS

[77 - Combinations](https://leetcode.com/problems/combinations/) 给定整数n和k，求在1-n中k个数的所有组合。

- DFS遍历，分别以1, 2, ..., n开头递归计算，时间复杂度\\( O(n!) \\)。

[39 - Combination Sum](https://leetcode.com/problems/combination-sum/) 给定整数数组C和目标T，求C中数字和为T的组合数，C中数字可重复使用。

- DFS遍历，时间复杂度\\( O(n!) \\)，空间复杂度\\( O(n) \\).

[301 - Remove Invalid Parentheses](https://leetcode.com/problems/remove-invalid-parentheses/) 给定包含字符串的字符串，从中去除最少的字符，使之括号有效匹配，并返回所有无重复结果。字符串中除'('，')'还包含其他字符。

- DFS，尝试删除每一个字符的可能性，生成结果后判断字符串的有效性，将其加入结果set；
- 生成结果时，记录结果的最长长度，最后根据最长长度对结果进行过滤；
- 剪枝：DFS时，若结果可能的最大长度小于当前最大长度，剪枝；

### BFS

---

## 9. 贪心

[3 - Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters/) 给定一个字符串，找出没有重复字符的最长字串的长度。

- 以哈希表记录每个字符出现的位置，此处哈希表用长度为256的字符数组代替；
- 变量start记录本次无重复字串的开始位置，遍历字符串，查找字符上次出现位置；
- 若出现在start之前，更新其位置，若出现在start之后说明重复，更新start为该字符上次位置的下一个，重新开始。复杂度\\( O(n) \\)。

---
