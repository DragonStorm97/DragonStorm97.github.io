+++
title = 'Reverse a Linked List'
date = 2024-10-03T22:39:44+02:00
draft = false
math = true
authors = ["Johnathan Jacobs"]
summary = "Reversing a Linked List in-place"
tags = ["linked-list", "reverse-linked-list"]
+++

**Problem**: Reverse a Linked list!

**Solution**: Store the `previous` and `next` pointers, as well as a `current` pointer.
Each iteration, we swap `current->next` to point to `previous`, where `next` is
used to temporarily store the original `current->next` to set `current` to.

**Why**: This uses no additional space, and does the swap in a single $O(n)$ pass.

```cpp
#include <memory>
struct LinkedNode{
  ListNode* next = nullptr;
};

[[nodiscard]] ListNode* reverseLinkedList(ListNode* head) {
  if(head == nullptr || head->next == nullptr) return head;
  ListNode* prev = nullptr;
  auto current = head;

  while (current != nullptr) {
    const auto next = current->next;
    current->next = prev;
    prev = current;
    current = next;
  }

  return prev;
}

int main() {
  auto n3 = std::make_unique<ListNode>();
  auto n2 = std::make_unique<ListNode>(n3.get());
  auto n1 = std::make_unique<ListNode>(n2.get());
  auto head = std::make_unique<ListNode>(n1.get());
  return reverseLinkedList(head.get()) == n3.get();
}
```
