+++
title = 'Quick Sort'
date = 2024-09-14T18:49:48+02:00
draft = false
math = true
tags = ["sort", "divide-and-conquer", "quick-sort"]
+++

An efficient, general purpose, comparison-based (total-order)
[Divide and Conquer]({{< relref "../divide-and-conquer.md" >}}) sorting algorithm.
Slightly faster than [Merge Sort]({{< relref merge-sort.md >}}).

Quick-sort works by selecting a 'pivot' element from the array and partitioning
the other elements into two sub-arrays, based on their value compared to the pivot.
This creates two sub-arrays where all elements in the first,
are is less than any element in the second.
The sub-arrays are then sorted recursively (can be done in-place).

Quick-sort is more of a description of a family of different sorts
which have different partitioning schemes.

Worst case performance: $O(n^2)$
Best, and average performance: $O(nlogn)$

## Algorithm

### In-Place Quick-Sort

1. If the range has fewer than two elements, return as there is nothing to do.
   Possibly, use a more special-purpose sorting method if there are only a few elements.
2. Otherwise, pick a value (the pivot) from the range (based off of the partitioning-scheme).
3. Partition the range: reorder its elements and choose a point of division. Put all elements smaller than the pivot
   before it and all elements larger after it.
4. Recursively quick-sort the sub-ranges up to the point of division, and to the sub-range after it.

### Lomuto Partition Scheme

Chooses the last element as the pivot.

```cpp
#include <vector>
#include <algorithm>

void quicksort(std::vector<int>& A, int lo, int hi) {
  if (lo >= hi || lo < 0) return;

  auto partition = [&](int lo, int hi) {
    int pivot = A[hi];
    int i = lo;
    for (int j = lo; j < hi; ++j) {
      if (A[j] <= pivot) std::swap(A[i++], A[j]);
    }
    std::swap(A[i], A[hi]);
    return i;
  };

  int p = partition(lo, hi);
  quicksort(A, lo, p - 1);
  quicksort(A, p + 1, hi);
}
//quicksort(A, 0, A.size()-1)
```

### Hoare Partition Scheme

1. Use two pointers into the range, that starts at both ends of the array.
2. Move the pointers toward each other until they detect an inversion
   (a pair of elements where the one at the first pointer is greater than the pivot,
   and the other at the second pointer is less than the pivot)
3. Exchange the values.
4. Move the pointers inwards again, and repeat the search for an inversion.
5. When the pointers eventually cross, a valid partition was found
   at the point of them crossing.

```cpp
#include <vector>
#include <algorithm>

void quicksort(std::vector<int>& A, int lo, int hi) {
  if (lo >= 0 && hi >= 0 && lo < hi) {
    auto partition = [&](int lo, int hi) {
      int pivot = A[lo];
      int i = lo - 1, j = hi + 1;

      while (true) {
        do { ++i; } while (A[i] < pivot);
        do { --j; } while (A[j] > pivot);

        if (i >= j) return j;
        std::swap(A[i], A[j]);
      }
    };

    int p = partition(lo, hi);
    quicksort(A, lo, p);
    quicksort(A, p + 1, hi);
  }
}
//quicksort(A, 0, A.size()-1)
```
