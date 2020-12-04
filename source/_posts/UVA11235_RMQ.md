title: UVA11235 - Frequent values
date: 2014-12-22 07:47:35
tags:
- ACM
- algorithm
- RMQ
category:
- 資料結構、演算法與ACM-ICPC
---

##[11235 - Frequent values](http://uva.onlinejudge.org/index.php?option=onlinejudge&page=show_problem&problem=2176)##

給你一個N個數字的遞增序列，接著給你M次詢問，每次詢問會給由兩個數字所代表的區間，你要輸出的答案就是在這段區間內出現最多
次的數字所出現的次數。

<!--more-->
輸入在N=0時結束。

###Sample Input###

```
10 3
-1 -1 1 1 1 1 3 10 10 10
2 3
1 10
5 10
0
```

###Sample Output###

```
1
4
3
```

算是相當經典的RMQ問題，雖然事實上不用RMQ暴力也可以過，只是時間非常難看，速度差了快10倍。

這裡使用Sparse-Table演算法(預處理O(nlogn)，查詢O(1))，一開始以為感覺不難，沒想到還卡了滿久的......

以這題而言，要先把相同的數字合在一起處理，用l(left)儲存左界，r(right)存右界，sum存出現次數。

簡單說明一下，程式裡的二維陣列rmq[i][j]是代表由編號i開始，長度為2的j次方的區間中出現最頻繁的次數。每個區間一定可以由
兩個rmq陣列所代表的區間所覆蓋，雖然會重複但不影響答案。

另外就是查尋的左右邊界不會剛好是不同數字的邊界，以這題來說要額外處理。

想更了解RMQ的可參考[維基](http://zh.wikipedia.org/wiki/%E8%8C%83%E5%9B%B4%E6%9C%80%E5%80%BC%E6%9F%A5%E8%AF%A2)

程式碼-RMQ版本(0.2秒多)：
```c++
#include <cstdio>
#include <cstring>
#include <algorithm>
 
#define MAX 100010
 
int group[MAX],l[MAX],r[MAX],sum[MAX],total,n,m;
int rmq[MAX][20];
 
void RMQ_init(){
	int i,j;
	memset(rmq, 0, sizeof(rmq));  
	for(i=1;i<=total;i++){
		rmq[i][0] = sum[i];
	}
	for(j=1; (1<<j) <= total; j++){
		for(i=1; i+(1<<j)-1 <= total; i++){
			rmq[i][j] = std::max(rmq[i][j-1], rmq[i+(1<<(j-1))][j-1]);  
		}
	}
}
 
int RMQ(int a, int b){
	int k=0,c=0,aa,bb;
	if(group[a] == group[b]){
		aa = bb = b-a+1;
	}
	else{
		aa = r[group[a]] - a +1;
		bb = b - l[group[b]] +1;
	}
	a=group[a]+1; b=group[b]-1;
	while(1<<(k+1) <= b-a+1)k++;
	if(b>=a)c = std::max(rmq[a][k], rmq[b-(1<<k)+1][k]);
	return(std::max(std::max(aa,bb),c));
}
 
int main(){
	int count,num,i,a,b,pre,ans;
	while(scanf("%d",&n)){
		if(n==0)break;
		pre=1;
		total=1;
		scanf("%d",&m);
		
		for(i=1;i<=n;i++){
			scanf("%d",&num);
			if(i==1){
				count = 1;
				l[total] = i;
			}
			else if(num>pre){
				r[total] = i-1;
				sum[total]= count;
				total++;
				count = 1;
				l[total] = i;
			}
			else{
				count++;
			}
			group[i] = total;
			pre = num;
		}
		r[total] = i-1;
		sum[total]= count;

		RMQ_init();

		for(i=0;i<m;i++){
			scanf("%d %d",&a,&b);
			ans = RMQ(a,b);
			printf("%d\n",ans);
		}
		memset(l,0,sizeof(l));
		memset(r,0,sizeof(r));
		memset(sum,0,sizeof(sum));
	}
	return 0;
}
```
暴力版本(2秒多)：

```c++
#include <cstdio>
#include <vector>
 
std::vector<int> l,r,sum;
 
int find(int a,int b){
	int ans=0;
	int low,up;
	low = (lower_bound(r.begin(),r.end(),a) - r.begin());
	up = (lower_bound(r.begin(),r.end(),b) - r.begin());
	if(up == low){
		a = b = sum[low] - (r[low]-b) - (a-l[low]);
	}
	else{
		a = r[low] - a +1;
		b = b - l[up] +1;
	}

	for(int i=low+1;i<up;i++){
		if(sum[i]>ans){
			ans = sum[i];
		}
	}
	if(ans>a && ans>b){
		return ans;
	}
	else if(a>b){
		return a;
	}
	else{
		return b;
	}
}
 
int main(){
	int m,n,count,num,i,a,b,pre;
	while(scanf("%d",&n)){
		if(n==0)break;
		pre=0;
		scanf("%d",&m);
		
		for(i=0;i<n;i++){
			scanf("%d",&num);
			if(i==0){
				count = 1;
				l.push_back(i);
			}
			else if(num>pre){
				r.push_back(i-1);
				sum.push_back(count);
				count = 1;
				l.push_back(i);
			}
			else{
				count++;
			}
			pre = num;
		}
		r.push_back(i-1);
		sum.push_back(count);

		for(i=0;i<m;i++){
			scanf("%d %d",&a,&b);
			printf("%d\n",find(a-1,b-1));
		}
		l.clear();
		r.clear();
		sum.clear();
	}
	return 0;
}
```
話說回來HEXO的程式碼顏色好像一直有點問題，看起來怪不舒服的......