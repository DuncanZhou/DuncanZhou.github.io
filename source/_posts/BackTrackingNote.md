---
title: 回溯法笔记
categories: Learning
---

# BackTracking Algorithm Notes
## 1.定义
> 在那些涉及寻找一组解的问题或者求满足某些约束条件的最优解的问题中，有许多问题可以用回溯法来求解。

为了应用回溯法，所要求的解必须能表示成一个n-元组（x1,x2,...,xn）,其中xi是取自某个有穷集Si。通常，所求解的问题需要求取一个使某一规范函数P(x1,...,xn)取极大值（或取极小值或满足该规范函数条件）的向量。有时还要找出满足规范函数P的所有向量。

## 2.基本思想
> 不断地用修改过的规范函数（或限界函数）Pi(x1,...,xn)去测试正在构造中的n-元组的部分向量(x1,...,xn)，看其是否可能导致最优解，如果不能，那么将可能要测试的其余向量略去，就是剪枝。

约束条件分为两类：显式约束和隐式约束。显示约束是限定每个x只从一个给定的集合上取值。隐式约束描述了xi必须彼此相关的情况，规定解空间中那些实际上满足规范函数的元组。
```
Algorithm backTracking
procedure PBACKTRACK(k)
//此算法是对回溯法抽象地递归描述。进入算法时，解向量X(1:n)的前k-1个分量X(1),...,X(k-1)已赋值
	global n,X(1:n)
	for 满足下式的每个X(k)
		X(k) 属于 T(X(1),...,X(k-1)) and B(X(1),...,X(k)) = True do
		if (X(1),...,X(k)) 是一条已抵达答案结点的路径 then
			print (X(1),...,X(k))
		endif
	call PBACKTRACK(k + 1)
	repeat
end PBACKTRACK
```
## 3.代表性的问题
### a、n-queen问题
有关n后问题的定义自行查阅，这里不给出解释。
<font color="green" style="font-weight: bold;">*解决思路*</font>
假定皇后i将放在行i上，因此，8皇后问题可以表示出8-元组(x1,x2,...,x8)，其中xi是放置皇后i所在的列号。隐式约束条件：1）没有两个xi可以相同；2）没有两个皇后可以在同一条对角线
假设两个皇后在(i,j)和(k,l)位置，则在同在对角线公式为abs(j-l) = abs(i-k)。即，在PLACE函数中可用此公式来判断。

```
procedure NQ(k)
//X(1),...,X(k-1)已赋值，求X(k),...,X(n)
	global X(1:n)
	X(k) = 1
	while X(k) <= n do
	//PLACE为检测第k个皇后是否满足约束条件
		if PLACE(k) then
			if k = n then 
				print (X(1),...,X(n))
			else
			//继续生成
				NQ(k + 1)
			endif
		endif
		X(k) = X(k) + 1
	repeat
end NQ
```
### b、子集和数问题
> 定义：已知n+1个正数：Wi,1<=i<=n和M。要求找出Wi的和数为M的所有子集。例如，若n=4，(W1,W2,W3,W4)=(11,13,24,7),M=31，则满足要求的子集是(11,13,7)和(24,7)。值得指出的是，通过给出其和数为M的那些Wi的下标来表示解向量比直接用这些Wi表示解向量更为方便。因此这个解向量由(1,2,4)和(3,4)表示。

显示约束条件要求Xi在1~n之间。隐式约束条件则是要求没有两个Xi是相同的且相应的Wi的和数是M。(为了避免产生同一个子集重复的情况,如(1,4,2)和(1,2,4))，附加另一个隐式约束条件:Xi+1大于Xi,i在1~n之间。
<font color="green" style="font-weight: bold;">*解决思路*</font>
```
    //current代表当前集合中已有和，k表示当前判断的第k个元素，rest代表集合中剩余元素之和
    public void sumOfsubset(int current,int k,int rest){
        //先生成左儿子,w[k]加入
        flag[k] = 1;
        if(current + w[k] == M){
            System.out.print("{\t");
            //子集找到，输出
            print(k);
            System.out.println();
        }
        else if(current + w[k] + w[k + 1] <= M)
            sumOfsubset(current + w[k],k + 1,rest - w[k]);
        //生成右儿子,w[k]不加入
        if((current + rest - w[k] >= M) && (current + w[k + 1] <= M)){
            flag[k] = 0;
            sumOfsubset(current,k + 1,rest - w[k]);
        }
    }
```
### c、图的着色
> 定义：已知一个图G和m>0种颜色，在只准使用这m种颜色对G的结点着色的情况下，是否能使图中任何相邻的两个结点都具有不同的颜色。这个问题称为m-着色判定问题。

<font color="green" style="font-weight: bold;">*解决思路*</font>
```
    	procedure MCOLORING(k)
		global integer m,n,X(1:n),boolean Graph(1:n,1:n)
		integer k
		loop
			//给第k个结点赋值颜色
			call NextValue(k)
			if X(k) = 0 then
				exit
			endif
			if k = n then
				print(X)
			else
				call MCOLORING(k + 1)
			endif
		repeat
	end MCOLORING
```
### d、哈密顿环
> 定义：一个哈密顿环是一条沿着图G的n条边环行的路径，它访问每个结点一次并且返回到它的开始位置。

### e、0/1背包问题
定义不解释，这个问题解决的方案很多，可以用动归、贪心算法，这里使用回溯法求解。
代码如下：
<font color="green" style="font-weight: bold;">*解决思路*</font>
```
package Chapter8;
//Created by duncan on 16-12-14.
public class BPByBackTrack {
    int bestvalue = 0;
    int[] lastfalg;
    public void swap(int[] x,int m,int n){
        int temp;
        temp = x[m];
        x[m] = x[n];
        x[n] = temp;
    }
    //先将物品效益和重量按照P/W的大小非递减排序,p和w都从1开始存储
    public void sort(int[] p,int[] w){
        int len = p.length -1 ;
        for(int i = 1; i <= len; i++){
            for(int j = 1; j <= len - i; j++){
                    if(((float)p[j] / w[j]) > ((float)p[j + 1] / w[j + 1])){
                        swap(p,j,j + 1);
                        swap(w,j,j + 1);
                    }
            }
        }
    }
    //限界函数,k为上次去掉的物品，M为背包容量,currentp为当前背包中效益值，currentw为背包中当前重量，返回值为当前最大值上限
    public int Bound(int[] p,int[] w,int k,int M,int currentp,int currentw){
        int tempp = currentp,tempw = currentw;
        int len = p.length - 1;
        for(int i = k + 1; i <= len; i++){
            tempw += w[i];
            if(tempw < M)
                tempp += p[i];
            else
                return (tempp + (1 - (tempw - M) / w[i] * p[i]));
        }
        return tempp;
    }
    //回溯法求解背包问题,p,w为效益值和重量数组，M为背包容量，k为当前处理的物品，flag为记录物品放或不放的标志数组,currentp为当前效益，currentw为当前重量
    public void BKNAPBT(int[] p, int[] w,int M,int k,int[] flag,int currentp,int currentw){
        int n = p.length - 1;
        if(k > n){
                if(currentp > bestvalue) {
                    bestvalue = currentp;
                    lastfalg = flag.clone();
                    return;
                }
        }
        else{
            //进入左子树
            if(currentw + w[k] <= M)
            {
                flag[k] = 1;
                BKNAPBT(p,w,M,k + 1,flag,currentp + p[k],w[k] + currentw);
            }
            //进入右子树前先判断右子树的最大上限是否能够比当前最优值大，如果没有则减去右子树
            if(Bound(p,w,k,M,currentp,currentw) >= currentp) {
                flag[k] = 0;
                BKNAPBT(p, w, M, k + 1, flag, currentp, currentw);
            }
        }
    }
    public void print(int[] p, int[] w){
        int len = p.length -1;
        int m = 0,v = 0;
        System.out.println("\n最终放入背包的物品的价值为:");
        for(int i = 1; i <= len; i++){
            if(lastfalg[i] == 1) {
                m += w[i];
                v += p[i];
                System.out.print(p[i] + " ");
            }
        }
        System.out.println("\n\n最终重量为：" + m);
        System.out.println("\n最优解价值为:" + v);
    }
    public static void main(String[] args) {
        int m = 0,M = 110;
        BPByBackTrack bp = new BPByBackTrack();
        int[] w = {0,1,11,21,23,33,43,45,55};
        int[] p = {0,11,21,31,33,43,53,55,65};
        int[] flag = {0,0,0,0,0,0,0,0,0};
        System.out.println("物品价值为");
        for(int i = 1 ; i <= p.length -1 ;i++){
            System.out.print(p[i] + "\t");
        }
        System.out.println("\n物品重量为");
        for(int i = 1 ; i <= p.length -1 ;i++){
            System.out.print(w[i] + "\t");
        }
        System.out.println();
        bp.sort(p,w);
        bp.BKNAPBT(p,w,M,1,flag,0,0);
        System.out.println("背包容量为:" + M);
        bp.print(p,w);
    }
}
```


