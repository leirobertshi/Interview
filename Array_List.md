# Array and List

Question  [Remove Duplicates from Sorted List I](https://leetcode.com/problems/remove-duplicates-from-sorted-list/)

```c++
ListNode* deleteDuplicates(ListNode* head) {
if (head == NULL) return NULL;
ListNode* point = head;
while (head->next!=NULL) {
	if (head->next->val == head->val) 
		head->next = head->next->next;
	else 
		head=head->next;
}
return point;
}
```

Question [Remove Duplicates from Sorted List II](https://leetcode.com/problems/remove-duplicates-from-sorted-list-ii)

```c++
ListNode* deleteDuplicates(ListNode* head) {
        if(!head) return head;
        ListNode dummy(0);
        dummy.next = head;
        ListNode *pre = &dummy;
        ListNode *cur = head;

        while(cur && cur->next){
            while(cur->next && cur->val == cur->next->val) cur = cur->next;
            if(pre->next == cur){
                pre->next = cur;
                pre = cur;
                cur = cur->next;
            } else {
                cur = cur->next;
                pre->next = cur;
            }
        }

        return dummy.next;
    }
```

Question Rotate List

```c++
ListNode *rotateRight(ListNode *head, int k) {
    if (head == nullptr || k == 0) return head;
    int len = 1;
    ListNode* p = head;
    while (p->next) { //
        len++;
        p = p->next;
    }
    k = len - k % len;
    p->next = head; //
    for(int step = 0; step < k; step++) {
    	p = p->next; //
    }
    head = p->next; //
    p->next = nullptr; //
    return head;
}
```

