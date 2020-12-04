title: ACM-ICPC LiveArchive 6187 <Never Wait for Weights>
date: 2014-10-17 22:52:55
tags:
- algorithm
- ACM
- disjoint set
category:
- 資料結構、演算法與ACM-ICPC
---

昨天去了ACM的團練，花了5個小時，一題都沒解出來......
上次系內賽都墊底了(連NCPC都去不了)，這次也那麼慘，看來真的緒要加油了QQ，最近一整個很混。
回去後至少拚了一題出來，其他題有空應該也會寫一寫吧。

<!--more-->

###題目###

[點我看原始題目](https://icpcarchive.ecs.baylor.edu/index.php?option=com_onlinejudge&Itemid=8&category=585&page=show_problem&problem=4198)

這裡稍微講下大意：

題目一開始會給你兩個數字n和m，n代表有n個物品(從1到n)，m代表有m筆資料要你的程式來處理，
這m筆資料會用驚嘆號(!)或問號(?)開頭。

如果驚嘆號開頭，會跟著三個數字，代表兩個物品間的重量關係，例如：
! 1 2 3
代表2號物品比1號物品重3單位。

而如果問號開頭，會跟著兩個數字，代表你得輸出兩個物品間的關係，例如：
? 1 2
你就得輸出1號和2號物品的重量關係，假設你知道2號比1號重3單位，就輸出3，
若之前給的資料不足以讓你回答他們的關係，就輸出UNKNOWN。

數字的範圍都在1到1,000,000之間。



Sample Input
```
2 2
! 1 2 1
? 1 2
2 2
! 1 2 1
? 2 1
4 7
! 1 2 100
? 2 3
! 2 3 100
? 2 3
? 1 3
! 4 3 150
? 4 1
0 0
```
Output for the Sample Input
```
1
-1
UNKNOWN
100
200
-50
```

###CODE###

```C++
#include <cstdio>
#include <algorithm>
#include <queue>
#include <cstring>

int root[100010]={0};
int dif[100010]={0};

bool flag;

int find_root(int a){
	std::queue<int> q;
	int r,rr,sum=0;
	r = a;
	while(root[r] != r){
		q.push(r);
		sum += dif[r];
		r = root[r];
	}
	rr = r;
	while(!q.empty()){
		r = q.front();
		q.pop();
		sum -= dif[r];
		dif[r] += sum;
		root[r] = rr; 
	}
	return root[a];
}

void merge(int a,int b,int w){
	int ra,rb;
	if(find_root(a) == find_root(b))return;
	else if(root[a] == a){
		root[a] = b;
		dif[a] = w;
	}
	else if(root[b] == b){
		root[b] = a;
		dif[b] = -w;
	}
	else{
		rb = find_root(b);
		ra = find_root(a);
		root[rb] = ra;
		dif[rb] = dif[a] - w - dif[b]; 
	}
	return;
}

int find(int a,int b){
	flag = true;
	return  dif[a] - dif[b];
}

int main(){
	int a,b,w,i,n,m,ans;
	char c;
	while(scanf("%d %d",&n,&m)!=EOF){
		if(n==0 && m==0)break;
		for(i=1;i<=n;i++){
			root[i] = i;
		}
		memset(dif,0,sizeof(dif));

		for(i=0;i<m;i++){
			scanf(" %c",&c);
			if(c == '!'){
				scanf("%d %d %d",&a,&b,&w);
				merge(a,b,w);
			}
			else{
				flag = false;
				scanf("%d %d",&a,&b);
				if(find_root(a) == find_root(b))ans = find(a,b);
				if(flag)printf("%d\n",ans);
				else printf("UNKNOWN\n");
			}
		}
	}
	return 0;
}
```

###解題概念###

團練當時我試圖用K次SPFA的最短路徑來求解，但是一直TLE，
後來才明白應該用disjoint set(並查集)的方法來做會比較有效率。

簡單來說，一開始每個物品都是自己一個集合，也就是root[n] = n，這裡的root代表自己的上一層元素。
假設後來我知道了2號比1號重3單位，我就把2號和1號加到同一個集合裡，這時，我可以選擇讓root[1]=2
或root[2]=1，總之root[1]要等於root[2]，這樣就知道1號和2號是在同個集合裡了。

再來，用dif這個陣列來存每個物品和root的重量差異，因為理論上在同個集合中的物品root會相等，所以
當題目問a和b的重量差時，如果a和b在同個集合中，就把dif[a](a和root的重量差)減掉dif[b](b和root的
重量差)，就可以知道a和b的重量差了。

反之，若a和b不在同一個集合裡，就可以直接輸出UNKNOWN，因為兩者並不相關。

比較複雜的演算法這裡就先不提(像是如何讓集合中的每個root一定相等)，有興趣或疑惑的可以上[演算法筆記](http://www.csie.ntnu.edu.tw/~u91029/Set.html#4)
看看更詳細的資料~~




