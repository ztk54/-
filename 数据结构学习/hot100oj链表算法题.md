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
## 反转链表
给你单链表的头节点 `head` ，请你反转链表，并返回反转后的链表。
这题我采用了自身原地反转的思想，使用pre，pcur，nextnode三个指针来实现自身原地反转，pcur指针遍历链表，pcur指向pre实现反转功能，nextnode保存下一个节点位置，实现遍历
```c
struct ListNode* reverseList(struct ListNode* head)
{
    if(head==NULL)
    {
        return head;
    }
    ListNode* pcur=head;
    ListNode* pre=NULL;
    ListNode* nextnode=head->next;
    while(pcur)
    {
        pcur->next=pre;
        pre=pcur;
        pcur=nextnode;
        if(nextnode)
            nextnode=nextnode->next;
    }
    return pre;
}
```
## 回文链表
给你一个单链表的头节点 `head` ，请你判断该链表是否为回文链表。如果是，返回 `true` ；否则，返回 `false` 。
这题我采用快慢指针找中间节点，根据中间节点反转后半段链表再与前面进行比较
```c
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     struct ListNode *next;
 * };
 */
typedef struct ListNode ListNode;
//快慢指针找到中间节点
ListNode* FindMid(ListNode* head)
{
    ListNode* slownode=head;
    ListNode* fastnode=head;
    while(fastnode&&fastnode->next)
    {
        slownode=slownode->next;
        fastnode=fastnode->next->next;
    }
    return slownode;
}
//原地反转链表
ListNode* ReverseList(ListNode* head)
{
    ListNode* pre=NULL;
    ListNode* pcur=head;
    ListNode* nextnode=pcur->next;
    while(pcur)
    {
        pcur->next=pre;
        pre=pcur;
        pcur=nextnode;
        if(nextnode)
        {
            nextnode=pcur->next;
        }
    }
    return pre;
}
bool isPalindrome(struct ListNode* head)
{
    ListNode* mid=FindMid(head);
    ListNode* right=ReverseList(mid);
    while(right)
    {
        if(head->val!=right->val)
        {
            return false;
        }
        else
        {
            head=head->next;
            right=right->next;
        }
    }
    return true;
}
```
> [!bug] 在这里的快慢指针循环条件中一定要是`while(fastnode&&fastnode->next)`不能颠倒顺序，不然会发生空指针的解引用。

