+++
title = 'Merge Sort'
date = 2024-09-14T18:49:48+02:00
draft = false
math = true
tags = ["sort", "divide-and-conquer", "merge-sort"]
+++

An efficient, general purpose, comparison-based
[Divide and Conquer]({{< relref "../divide-and-conquer.md" >}}) sorting algorithm.

## Algorithm

- Divide the unsorted list into $n$ sublists,
  each containing one element. A list of one element is considered sorted.

- Repeatedly merge sublists to produce new sorted sublists, until only one list remains.

## Top-down implementation

This implementation is $O(nlogn)$ as every iteration on $n$ causes work on half as much $n$.

```cpp
std::vector<int> TopDownMergeSort(std::vector<int> input) {
  auto work = input;
  TopDownSplitMerge(input, 0, input.size(), work);
  return input;
}

void TopDownSplitMerge(std::vector<int>& in, std::size_t begin, std::size_t end, std::vector<int>& wrk) {
  if(end - beging <= 1) return;

  std::size_t mid = (end + begin) / 2;
  // split the input in two
  TopDownSplitMerge(wrk, begin, mid, in); // sort the left run
  TopDownSplitMerge(wrk, mid, end, in);   // sort the right run
  // merge the runs back
  TopDownMerge(in, begin, mid, end, wrk);
}

void TopDownMerge(std::vector<int>& in, std::size_t beg, std::size_t mid, std::size_t end, std::vector<int>& wrk) {
  std::size_t i = beg;
  std::size_t j = mid;
  // for each output element, choose either the left or the right input in order
  for (auto k = beg; k < end; ++k) {
    if (i < mid && (j >= end || wrk[i] <= wrk[j])) {
      in[k] = wrk[i];
      ++i;
    } else {
      in[k] = wrk[j];
      ++j;
    }
  }
}
```

## List-Based Implementation

More efficient than array-based as we can just swap pointers.
Though, think of the cache misses!
