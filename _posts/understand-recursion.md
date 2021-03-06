---
title: 递归、搜索与回溯
date: 2017-04-01 23:09:07
---
同学对于递归一直不能理解，借自己的一臂之力帮帮他吧。

### 递归？循环？

首先，递归就是：**不断的重复自己**。

这里有一个奇怪的故事：

> 很久以前，有一位老爷爷讲了一个故事，故事讲了很久以前，有一位老爷爷讲了一个故事，故事讲了……

我们很容易看出，这个故事就是“很久以前，有一位老爷爷讲了一个故事，故事讲了”这句话不断重复。

> 我吃饭了，吃完饭了。我吃饭了，吃完饭了。……

这个例子同样的是由一句话重复得来的。

但不同的是，第二次吃饭与第一次吃饭是独立的，做完这个再接着做那个，是广度的重复，行为类似于循环。

第一个例子，是在讲故事的过程中又讲了一个故事，两件事具有从属关系，是**深度的重复，这才是我们讲的递归**。

### 递归的边界条件

定义很简单，就是不断的重复自己。但在实际的使用中，肯定不能无限的重复，**任何递归都存在一个停止的情况（边界条件）**。

来个例子吧，来自“《Thinking In Algorithm》09.彻底理解递归”。

> 阶乘的递归实现：

```
int factorial(int N) {   
	if (N == 1)
		return 1;   
	return N * factorial(N-1);   
}  
```

数学上的递推式：`f(x)=x*f(x-1),f(1)=1`

在这里，边界条件就是，当`N==1`时返回1并停止递归。

### 递归的过程

程序运行的步骤如下：

```
factorial(4)    
    =   4 * factorial(3)  
    =   4 * (3 * factorial(2) )  
    =   4 * (3 * (2 * factorial(1) ) )  
    =   4 * (3 * (2 * 1) )  
    =   4 * (3 * 2)  
    =   4 * 6  
    =   24  
```

也可以是

```  
factorial(4)   
  factorial(3)   
    factorial(2)   
      factorial(1)   
        return 1   
      return 2*1 = 2   
    return 3*2 = 6   
  return 4*6 = 24   
```

4-> 一开始，我们调用的是factorial(4)，进入该函数，发现需要提供一个返回值，该返回值来自`N*factorial(N-1)`，也就是`4*factorial(3)`。

3-> 那么factorial(3)的值是多少呢？进入factorial(3)，发现它的值其实来自`3*factorial(2)`。

2-> 聪明的你很快就能想到，factorial(2)又是来自`2*factorial(1)`。

1-> 运行到factorial(1)，发现if语句控制的边界条件满足了，直接返回一个1并结束factorial(1)。

2-> 这时视线回到factorial(2)，它的返回值现在确定了，是`2*1=2`，于是返回2并结束factorial(2)

3-> 那么factorial(2)结束后，程序又回到调用factorial(2)的地方，也就是factorial(3)。这时factorial(3)的值因为factorial(2)的确定，也确定了，它的值是`3*factorial(2)=3*2=6`。

4-> 再回到一开始的factorial(4)，他的值因为factorial(3)的确定，也得到了确定，是`4*6=24`，这时递归程序就得到了factorial(4)的值，也就是4！的值，等于24。

上面这一大段文字就是程序运行的步骤的详解，在每段话的左边我用数字作出了标记，代表的是当前的程序运行到的步骤。

而虽然每次乘法的值都不同，但其实**不管是factorial(4)还是factorial(3)还是factorial(2)甚至是factorial(1)，他们的程序都是一模一样的，都由一开始的那段程序控制，而乘法的值不同只取决于参数N的不同而不同**，所以我们说这是一个递归的例子。

### 递归的变量问题

经过我这么这么一大段的废话之后，你可能已经对递归有了一个初步的理解了（没有的话面壁3s后再看一遍……），或许这个东西你能用自然语言描述出来，但是在编程语言中还是有很多的点没有弄清……

比如，同样一个变量N，N的值在factorial(1)改变了之后，为什么在factorial(4)里还是原来的值呢？

我们看看下面两个函数。

```
void a() {
  int k=1;
  b();
  cout << k << endl;//这里输出多少呢？
}
void b() {
  int k=2;
  cout << k << endl;//这里输出多少呢？
}
```

其实如果你基础好的话很容易得出结果，a函数输出的1，b函数输出的2，原因很简单，因为k是个局部变量，a中的k其实是ka，b中的k其实是kb，是两个不同的变量，互不干扰。

```
int k;
void a() {
  k=1;
  b();
  cout << k << endl;//这里输出多少呢？
}
void b() {
  k=2;
  cout << k << endl;//这里输出多少呢？
}
```

而在这个例子中就都是2，因为k是个全局变量，不管在a里面还是在b里面，都是同一个b。在b中修改了k=2，那回到a还是2。

对于递归来说，其实在factorial(4)中的N，是属于factorial(4)的，与factorial(1)的、factorial(2)、factorial(3)的变量无关，只是相当于b()替换成了factorial(3)、c()变成了factorial(2)……

**所以在factorial(1)中运行完后，一切变量等等的状态会原样返回到factorial(2)，factorial(2)得到值后，factorial(3)的状态又原样被拿出来，并不受factorial(2)的影响，保持与调用factorial(2)之前一致。**

### 递归&循环

再来说一下递归与循环的区别吧。

循环是广度的，每一次循环体执行完了后才会执行下一次的循环，**每一次的循环都是并列的关系**。

而递归不同，每一次的递归是具有深度的，是在每一次递归的中间又执行下一次的递归，依次递归下去又返回回来，继续递归之后的代码。

**每一次的递归都是层叠、嵌套的关系**。

所以，递归一般用来解决，有多层的嵌套关系的问题，比如全排列（见下文）。

### 递归总结

最后概括一下，递归算法，总结起来具有以下几个特点（摘至“C++递归算法：我的理解”）：

1. 它有一个基本部分，即直接满足条件，得到结果（边界条件）
2. 它有一个递归部分，即 通过改变基数（即n），来逐步使得n满足基本部分的条件，从而得到结果
3. 每一步操作，整体都会将部分当作其必要的一个步骤，从而实现整体步骤的完成

> 引用资料：http://blog.csdn.net/jinghouxiang/article/details/50583819
http://blog.csdn.net/speedme/article/details/21654357

### 搜索算法

终于讲完递归了，妈妈咪啊……这么长的废话。

让我们来看一道简单的题：

> 给定一个整数N，输出1-N的全排列

若N=3时，也就是1-3的全排列，答案应该是123,132,213,231,312,321

这时我们的代码很好写：

```
for (int i=1;i<=3;i++)
  for (int j=1;j<=3;j++)
    for (int k=1;k<=3;k++)
      if (i!=j && i!=k && j!=k)
        printf("%d %d %d\n", i, k, j);
```

随意暴力搞搞就行。但是，N会变啊！N==3时，就是3个for循环嵌套，N==10时就是10个for循环嵌套……先不说可行性，那代码得有多丑！

基于我们之前研究得到的递归的特性，可以采用这样的方案：

```
if (当前递归层数==N+1) //为什么是N+1？自行思考
  返回
for i=1到N
{
  输出i
  递归（当前层数+1）
}
```

这样就解决了for循环嵌套过多的问题，每一层所输出的数就是他取的数。

但是，我们的题目要的是全排列啊，还没有解决每一位上的数不能相同的问题呀。这里怎么判断呢？

在上面的例子我们是判断几个循环变量全部不同来判断的。但在这里选择了就输出了，N大了几十个循环变量全部判断一遍一个也不现实（每两个就要判断一次……）

我们可以采用用一个数组记录下来哪些数选过，递归到下一层的时候判断一下，选过的跳过不选就行了。

基于这个思路，可以完善一下程序。

```
if (当前递归层数==N+1) //为什么是N+1？自行思考
  返回
for i=1到N
{
  if（当前i用过）//因为全排列中每个数只能出现一次
    跳过当前循环
  输出i
  记录i用过
  递归（当前层数+1）
  还原i变成没用过 //关键，为什么要还原一下？
}
```

这样就构成了一个完整的搜索与回溯结构。

在代码中，有一个关键点，在递归过后我们还原了一下使用的状态，为什么呢？

因为如果不还原，用过一次就再也不能用了，那么第一层递归输出1，第二层2，第三层3，然后就没有数可用了……

为了还能有数在返回后继续递归，用了之后还要“还回去”。而还回去的这个过程，就叫做回溯。

那么，翻译一下这个标准的搜索、回溯结构，编程C++语言就可以解出全排列了。

```
bool book[25];
int n;
void dfs(int step)
{
  if (step==N+1) //step代表的是当前的递归的层数。dfs(1)就代表在选第一个数，dfs(2)就是在选第二个数
    return;
  for (int i=1;i<=n;i++)
  {
    if (book[i])
      continue;
    cout << i;
    book[i]=true;
    dfs(step+1); //递归下一层
    book[i]=false; //回溯
  }
}
```

其中book是一个bool数组，如果book[x]是true，就代表x这个数在之前的层数被用过了。

看到这里，其实搜索、回溯已经讲完了，很意外么？

当然，沿着我的思路走过来或许会觉得搜索回溯的代码是这么顺理成章，可自己要直接看这段代码，或许就会觉得很难以理解，觉得自己肯定想不到这样做。

这时，返回去再看一遍，揣摩每一句代码的含义在哪里，递归这么写的优势在哪，很快就能理解的。
