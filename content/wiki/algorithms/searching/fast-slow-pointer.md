+++
title = 'Fast & Slow Pointer'
date = 2024-10-03T21:30:51+02:00
draft = false
math = true
authors = ["Johnathan Jacobs"]
summary = "Technique to detect cycles, duplicates or other relationships in a container."
tags = ["search", "traverse", "dynamic-programming", "sliding-window", "fast-slow-pointer", "two-pointers", "tortoise-and-hare", "cycle-detection", "remove-duplicates", "linked-list"]
+++

The `fast and slow pointer` technique (aka "tortoise and hare" algorithm) is
useful for problems where you need to detect cycles, or relationships in a container.

The algorithm involves two pointers into the iterable, one `fast` that moves
through the data faster than the other (`slow`). This technique is also used for
some solutions as an optimization to solve a problem in linear time.

## Use Cases

### Cycle Detection in a Linked List (Floyd's Cycle Detection Algorithm)

**Problem**: Determine if a given linked list contains a cycle (a sequence of nodes
form a loop back to an earlier node).

**Solution**: Use two pointers: one moves one step at a time (`slow`), and the
moves two steps at a time (`fast`). If there is a cycle, the `fast`
pointer will eventually pass `slow` pointer in the cycle.

**Why**: This approach detects cycles in $O(n)$ time and uses $O(1)$ space, as the
two pointers move through the list without requiring additional data structures.
A brute-force approach would require $O(n^2)$ time or additional space
(like a hash set) to track visited nodes.

```cpp
#include <memory>

// Basic singly-linked list with no data
struct ListNode {
  ListNode* next = nullptr;
};

[[nodiscard]] bool hasCycle(ListNode* head) {
  if (head == nullptr || head->next == nullptr) return false;
  ListNode* slow = head;
  ListNode* fast = head->next;
  while (fast != nullptr && fast->next != nullptr) {
    if(slow == fast) return true;
    slow = slow->next;
    fast = fast->next->next;
  }
  return false;
}

int main() {
  auto n3 = std::make_unique<ListNode>();
  auto n2 = std::make_unique<ListNode>(n3.get());
  auto n1 = std::make_unique<ListNode>(n2.get());
  // add cycle:
  n3->next = n1.get();
  auto head = std::make_unique<ListNode>(n1.get());
  return hasCycle(head.get()) ? 1 : 0;
}
```

### Finding the Middle of a Linked List

**Problem**: Find the middle node of a singly linked list in one traversal.

**Solution**: Use the fast pointer to move two steps at a time and the slow
pointer to move one step at a time. When the fast pointer reaches the end,
the slow pointer will be at the middle.

**Why**: This only requires one pass through the list $O(n)$, whereas multiple
loops or tracking the length separately would require $O(n)$ space or multiple
passes.

```cpp
#include <memory>
struct ListNode {
  ListNode* next = nullptr;
};
[[nodiscard]] ListNode* findMiddle(ListNode* head) {
  if (gead == nullptr) return nullptr;
  auto slow = head;
  auto fast = head;
  // we are done if fast was the last element
  while (fast != nullptr && fast->next != nullptr) {
    slow = slow->next;
    fast = fast->next->next;
  }
  return slow;
}

int main(){
  auto n4 = std::make_unique<ListNode>();
  auto n3 = std::make_unique<ListNode>(n4.get());
  auto n2 = std::make_unique<ListNode>(n3.get());
  auto n1 = std::make_unique<ListNode>(n2.get());
  auto head = std::make_unique<ListNode>(n1.get());
  return findMiddle(head.get()) == n2 ? 1 : 0;
}
```

### Measuring the Length of a Cycle in a Linked List

**Problem**: After detecting a cycle in a linked list, determine the length of
the cycle.

**Solution**: Once a cycle is detected using the fast and slow pointers, keep one
pointer in place and move the other until they meet again, counting the steps.

**Why**: After detecting the cycle, the fast and slow pointers help calculate the
cycle length efficiently in $O(n)$ time, without needing extra space.

```cpp
#include <memory>

struct ListNode {
  ListNode* next = nullptr;
};

[[nodiscard]] int cycleLength(ListNode* head) {
  auto slow = head;
  auto fast = head;
  while (fast != nullptr && fast->next != nullptr) {
    slow = slow->next;
    fast = fast->next->next;
    if(slow == fast) {
      // we detected a cycle, find the length
      int length = 0;
      do {
        ++length;
        slow = slow->next;
      } while (slow != fast);
      return length;
    }
  }
  return 0;
}

int main() {
  auto n3 = std::make_unique<ListNode>();
  auto n2 = std::make_unique<ListNode>(n3.get());
  auto n1 = std::make_unique<ListNode>(n2.get());
  // add cycle:
  n3->next = n1.get();
  auto head = std::make_unique<ListNode>(n1.get());
  return cycleLength(head.get()) == 3;

}
```

### Detect the Starting Point of a Cycle in a Linked List

**Problem**: Given a linked list with a cycle, find the node where the cycle
begins.

**Solution**: After detecting a cycle, reset one pointer to the head of the list.
Move both pointers one step at a time. When they meet again, the meeting point
is the start of the cycle.

**Why**: The two-pointer method finds the starting node of the cycle in $O(n)$ time
with $O(1)$ space, avoiding the need for additional data structures.

```cpp
#include <memory>

struct ListNode {
  ListNode* next = nullptr;
};

[[nodiscard]] ListNode* detectCycleStart(ListNode* head) {
  auto fast = head;
  auto slow = head;
  while (fast != nullptr && fast->next != nullptr) {
    fast = fast->next->next;
    slow = slow->next;
    if (slow == fast) {
      // we've detected a cycle
      slow = head;
      while (slow != fast) {
        slow = slow->next;
        fast = fast->next;
      }
      return slow; // start of cycle
    }
  }
  return nullptr;
}

int main() {
  auto n3 = std::make_unique<ListNode>();
  auto n2 = std::make_unique<ListNode>(n3.get());
  auto n1 = std::make_unique<ListNode>(n2.get());
  // add cycle:
  n3->next = n1.get();
  auto head = std::make_unique<ListNode>(n1.get());
  return detectCycleStart(head.get()) == n1.get();
}
```

### Remove Duplicates from a Sorted Array

**Problem**: Remove duplicates in-place from a sorted array.

**Solution**: Use one pointer (`slow`) to track the position of the next unique
element, and another (`fast`) to scan through the array.
If `nums[fast]!=nums[slow]`, move `slow` slow forward and copy `nums[fast]` to
`nums[slow]`.
This will keep all the unique elements in the front of the array, and return an
index to the last unique element.

This method removes duplicates in $O(n)$ time through one pass, instead of
comparing each element to every other $O(n^2)$.

```cpp
#include <vector>

[[nodiscard]] std::size_t removeDuplicatesSorted(std::vector<int>& nums) {
  if(nums.empty()) return 0;
  std::size_t slow = 0;
  const auto size = nums.size();
  // since we never actually use `fast` we can just iterate over the values directly
  for(auto num : nums) {
    if(num != nums[slow]) {
      ++slow;
      nums[slow] = num;
    }
  }
  // return the length of the array with unique elements
  return slow + 1;
}
```

### Finding Duplicate numbers in (a very specific) Array (with Cycle Detection)

**Problem**: Given an array of n+1 integers where each integer is between 1 and n,
there is exactly one duplicate number. Find the duplicate using constant space
and less than $O(n^2)$ time.

**Solution**: Treat the array as a linked list, where each value in the array is
a pointer to another index (i.e., `nums[i]` points to `nums[nums[i]]`).
This creates a cycle. Use the fast and slow pointer technique to detect the duplicate.

**Why**: This algorithm solves the problem in $O(n)$ time and $O(1)$ space, whereas
a naive approach might involve sorting the array ($O(n log n)$) or using a hash
table ($O(n)$ time but $O(n)$ space). By leveraging the array’s structure as a
cycle, the fast-slow pointer method finds the duplicate efficiently.

```cpp
#include <vector>
// NOTE: this assumes there is exactly one duplicate!
[[nodiscard]] int findDuplicate(const std::vector<int>& nums) {
  auto slow = nums[0];
  auto fast = nums[slow];
  while (slow != fast) {
    slow = nums[slow];
    fast = nums[nums[fast]];
  }
  slow = 0;
  // find the start of the cycle (the duplicate number)
  while (slow != fast) {
    slow = nums[slow];
    fast = nums[fast];
  }
  return slow;
}

int main() {
  return findDuplicate({1, 2, 3, 4, 5, 2, 7}) == 2;
}
```

### Rearrange a Linked List by Even and Odd Positions

**Problem**: Given a linked list, rearrange the nodes such that all nodes at odd
positions come before all nodes at even positions.

**Solution**: Use two pointers: one to track the odd-indexed nodes and the other
to track the even-indexed nodes. Move both pointers in one pass through the list.

**Why**: This technique rearranges the nodes in $O(n)$ time and $O(1)$ space,
without requiring extra storage or additional passes.

```cpp
#include <memory>

struct ListNode {
  ListNode* next = nullptr;
};

[[nodiscard]] ListNode* oddEvenList(ListNode* head) {
  if (head == nullptr || head->next == nullptr) return head;
  auto odd = head;
  auto even = head->next;
  auto evenHead = even;

  while(even != nullptr && even->next != nullptr) {
    odd->next = even->next;
    odd = odd->next;
    even->next = odd->next;
    even = even->next;
  }
  odd->next = evenHead;
  return head;
}

int main() {
  auto n3 = std::make_unique<ListNode>();
  auto n2 = std::make_unique<ListNode>(n3.get());
  auto n1 = std::make_unique<ListNode>(n2.get());
  auto head = std::make_unique<ListNode>(n1.get());
  return oddEvenList(head.get())->next == n2.get();
}
```
