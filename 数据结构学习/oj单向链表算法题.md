## 返回相交链表节点
*方法一*两个链表较长的减去较短的，然后让较短的先走n步，然后再比较地址是否相等
```c
struct ListNode *getIntersectionNode(struct ListNode *headA, struct ListNode *headB)

{
    ListNode* pa=headA;
    ListNode* pb=headB;
    int sizeA=0;
    int sizeB=0;
    while(pa)
    {
        pa=pa->next;
        sizeA++;
    }
    while(pb)
    {
        pb=pb->next;
        sizeB++;
    }
    int gap=abs(sizeA-sizeB);
    ListNode* longNode=headA;
    ListNode* shortNode=headB;
    if(sizeA<sizeB)
    {
        longNode=headB;
        shortNode=headA;
    }
    while(gap)
    {
        longNode=longNode->next;
        gap--;
    }
    while(shortNode)
    {
        if(shortNode==longNode)
        {
            return shortNode;
        }
        shortNode=shortNode->next;
        longNode=longNode->next;
    }
    return NULL;
}
```
一些问题思考，第一是错误使用了long和short作为变量名，但是这两个是c语言关键字，不能作为变量名称使用，第二是可以直接使用绝对值函数abs
*方法二：双指针法*核心思路就是让两个指针循环前进，一旦走到末尾就交换到另一条链表的头节点，原理是假如有链表A和B，A的单独部分设为a，B的公共部分设为b，公共部分设为c，第一遍走到NULL的时候A走了a+b，B走了b+c，如果*两者相交的话*，那么A再走c步，B再走a步，必定会有相遇。如果两者*没有相交*的话，那么就是A会走a+b步，B会走a+b步，同时指向NULL，退出循环。
```c
struct ListNode *getIntersectionNode(struct ListNode *headA, struct ListNode *headB)
{
    if(headA==NULL||headB==NULL)
    {
        return NULL;
    }
    ListNode* pa=headA;
    ListNode* pb=headB;
    while(pa!=pb)
    {
        pa=(pa==NULL)?headB:pa->next;
        pb=(pb==NULL)?headA:pb->next;
    }
    return pa;
}
```
这里在循环那里犯了一个很致命的错误，`pa=(pa==NULL)?headB:pa->next;pb=(pb==NULL)?headA:pb->next;`这里原本的headA和headB写的是pa和pb，但是这样就导致了逻辑混乱，比如说pa直接指向pb的位置了，而不是指向头节点的位置。
