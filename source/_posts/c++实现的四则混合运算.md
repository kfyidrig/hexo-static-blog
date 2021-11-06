---
title: c++实现的四则混合运算
date: 2021-03-23 16:50:42
tags: [刷题记录]
---

C++封装的四则混合运算类Calculator，通过`addProps(char)`方法添加的表达式支持小括号，传参`string`类型**不支持括号**，还有很多需要完善的地方，暂时放这儿吧。

<!-- more -->

```c++
#include<iostream>
#include <stack>
#include <queue>
#include <sstream>
using namespace std;
class Calculator{
public:
    int lastValue;
    void addProps(int tmp){
        if(tmp*-1 ==')'){
            //如果是右扩符，出操作符栈进后缀队列，直到遇到左括符
            while (!op.empty()){
                int numOp=op.top(); op.pop();
                if(numOp*-1=='(') return;
                raw.push(numOp);
            }
} else if(tmp<0){
            //如果是操作符
            char ch=-1*tmp;
            //操作符栈非空，栈顶元素优先级高于或者等于新操作符
            while (!op.empty()&&judgePri(ch,op.top()*-1)<1){
                raw.push(op.top());
                op.pop();
            }
            op.push(tmp);
        } else raw.push(tmp);   //数字直接后缀队列
    }

    void addProps(string s){
        stringstream ss(s);
        int curNum; ss>>curNum;
        char ch;    raw.push(curNum);
        while (ss>>ch){
            addProps(-1*ch);
            ss>>curNum; raw.push(curNum);
        }
    }

    int getValue(){     //获取输入的表达式的值
        while (!op.empty()){
            raw.push(op.top());
            op.pop();
        }
        while (!raw.empty()){
            int tmp=raw.front();
            if(tmp>-1) num.push(tmp); else {
                char ch=-1*tmp;
                int t2=num.top(); num.pop();
                int t1=num.top(); num.pop();
                if(ch=='+') t1+=t2;
                else if(ch=='-') t1-=t2;
                else if(ch=='/') t1/=t2;
                else t1*=t2;
                num.push(t1);
            }
            raw.pop();
        }
        lastValue=num.top();    num.pop();
        return lastValue;
    }
private:
    queue<int> raw;
    stack<int> op;
    stack<int> num;
    int judgePri(char a,char b){
        if(a=='('||b==')') return 2;    //如果为括符，返回2
        //如果a为最高优先级，对b进行判断
        else if(a=='*'||a=='/') { if(b=='-'||b=='+') return 1; else return 0;}
        //如果a为+或者-，判断b的优先级是否高于a
        else if(b=='*'||b=='/') return -1;
        return 0; //默认返回同等优先级0
    }


};
int main(){
    Calculator tmp;
    tmp.addProps("1+1*9+10+30");
    cout<<tmp.getValue();  //输出50
    return 0;
}

```

