# 链表和递归

### LeetCode中链表的相关问题

删除链表中等于给定值val的所有元素。

**示例**

给定：1-->2-->6-->3-->4-->5-->6 ,val =6

返回：1-->2-->3-->4-->5

**解题思路：**调用在LeetCode中定义好的LinkedList类

1，首先要求头结点不是空的，如果头节点中包含val，则直接删除，**但是要用循环**，因为head.next和之后的Node中也许会还包含指定元素

2，如果链表中从开始到最后都是要删除的节点，则直接返回空。

3，找到待删除元素的前一个节点。因为此时的，head肯定不为空，所以把prev设为head。

4，只要该节点（prev）的下一个节点（prev.next）不为空，如果有包含val的节点。则删除该节点；否则prev向后移动一位。

5，结束所有的操作后最后返回head节点。

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public ListNode removeElements(ListNode head, int val) {//传入头结点和要删除元素所包含的节点。 
        while(head != null && head.val == val){
            ListNode delNode = head;  //定义待删除的节点
            head = head.next;         //绕过我们要删除的节点
            delNode.next = null;      
        }
        if(head == null)
            return null;              //也许该链表中所有节点都是要被删除的节点
        
        ListNode prev = head;         //找到待删除元素前一个节点
        while(prev.next ！= null){    
            if(prev.next.val == val){ //删除prev.next但是prev不用向后挪。
                ListNode delNode = prev.next;
                prev.next = delNode.next;
                delNode = null;
            }
            else
                prev = prev.next;    //prev向后挪一位
        }
        
        return head;
    }
}
```

