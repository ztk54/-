# 返回相交链表节点
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
