

### vector

```C++
//Eg7-13.cpp
#include<iostream>
#include<vector>                  		//向量头文件
using namespace std;

void display(vector<int> &v) {         	
		while(!v.empty()){
			cout<<v.back()<<"\t";   //输出向量的尾部元素
			v.pop_back();            //删除向量尾部元素
		}
		cout<<endl;
}
void main(){
		int a[]={1,2,3,4,5,6};
		vector<int> v1, v2;             	//定义只有0个元素的向量v1、v2
		vector<int> v3(a,a+6);          	//定义向量v3，并用a数组初始化该向量
		vector<int> v4(6);              	//定义具有6个元素的向量v4
		v1.push_back(10);             	//在v1向量的尾部加入元素10
		v1.push_back(11);
		v1.push_back(12);
		v1.insert(v1.begin(),30);    	//将30插入到v1向量的最前面
		v2=v1;                       	//将v1赋值给v2，v2与v1具有相同的元素
  		v3.assign(3,10);                	//将v3的前3个元素都设置为10
		cout<<"v1: "; display(v1);
		cout<<"v2: "; display(v2);
		cout<<"v3: "; display(v3);
		v4[0]=10; v4[1]=20;		//用数组方式访问向量元素
		v4[2]=30; v4[3]=40;   
		cout<<"v4: ";
		for(int i=0;i<6;i++)
		cout<<v4[i]<<"\t";
		cout<<endl;
		v4.resize(10);       	               //重置向量v4的大小，已有元素不受影响
		cout<<"v4: "; display(v4);
}

```



