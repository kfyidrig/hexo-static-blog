---
title: 仿真链表k个一组反转链表
date: 2021-01-27 12:42:59
tags: [刷题记录]
---

<p>PAT乙级k个一组反转链表有两种方式，第一种是用仿真链表k个一组反转链表，第二个是直接用链表，下面用仿真链表来实现，最后附上真实链表的代码。</p>
<p><strong>思路：先学会反转整个链表，然后反转指定节点之间的链表，例如链表 a - b - c ,只反转成b - a - c,这样递归下去就能实现反转整个链表咯</strong></p>
<h3>链表结构体</h3>
```c++
struct node{
    int next;
    int data;
}
```

<!-- MORE -->

<h3>反转指定节点段</h3>

//反转节点a到b之间的节点，返回新头节点，此时b成为独立的链表

```c++
int Reverse(int a,int b,node link&#91;]){
    int pre, cur, nxt;
    pre=-1; cur=a; nxt=a;
    while (cur != b) {
        nxt=link&#91;nxt].next;  //下一个节点移动到当前的下一个
        link&#91;cur].next = pre; //当前节点等于前一个
        pre = cur;
        cur = nxt;
    }
    return pre;
}
```



<h3>K个一组反转</h3>
```c++
int reverseKGroup(int head,int k,node link&#91;]) {
    if(head==-1) return -1;
    int a, b;
    a = head;
    b = head;
    for (int i = 0; i &lt; k; i++) {
        if (b == -1) return head;
        b = link&#91;b].next;       //找到k的下一个节点
    }
    int newHead = Reverse(a, b,link);
    link&#91;a].next = reverseKGroup(b,k,link); //递归并连接所有节点
    return newHead;
}
```

