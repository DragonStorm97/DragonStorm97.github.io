+++
title = 'Select K Elements Pattern'
date = 2024-10-05T23:03:56+02:00
draft = false
math = true
authors = ["Johnathan Jacobs"]
summary = "A pattern to efficiently find the K-largest/smallest/most-frequent elements in an array using a heap."
tags = ["search", "traverse", "heap", "smallest-element", "largest-element", "min-heap", "max-heap"]
+++

Instead of sorting an array, and selecting the elements ($O(nlogn)$),
we use a min or max [heap]({{< relref heaps>}}) to keep track of the `k` elements.

## Methods

### K-largest Elements in an Array

**Problem**: Given an array of `n` integers, return the `k` largest elements.

**Solution**: Use a [min-heap]({{< relref heaps >}}) of size `k` to keep track of
the `k` largest elements. As we iterate through the array, if an element is
larger than the root of the heap (the smallest of the `k` largest elements),
we replace the root with the current element.

**Why**: The min-heap ensures that the smallest of the K largest elements
is at the root. As we process each element, we only update the heap if the
new element is larger than the current minimum. This approach runs in $O(n log k)$
time compared to the $O(nlogn)$ time required to sort the entire array.

```cpp
#include <vector>
#include <algorithm>
#include <iostream>

[[nodiscard]] std::vector<int> kLargestElements(const std::vector<int>& nums,
                                                std::size_t k) {
  const auto size = nums.size();
  // Initial heap of size K
  std::vector<int> minHeap(nums.begin(), nums.begin() + k);
  // Create a min-heap
  std::make_heap(minHeap.begin(), minHeap.end(), std::greater<>{});

  for (std::size_t i = k; i < size; ++i) {
    if (nums[i] > minHeap.front()) {
      // Remove the smallest element (actually just moves the largest to the end)
      std::pop_heap(minHeap.begin(), minHeap.end(), std::greater<>{});
      // Replace with the new larger element
      minHeap.back() = nums[i];
      // Restore heap property
      std::push_heap(minHeap.begin(), minHeap.end(), std::greater<>{});
    }
  }

  // The heap now contains the K largest elements
  return minHeap;
}

int main() {
  auto largest = kLargestElements({3, 2, 1, 5, 6, 4}, 2);
  // Now the largest contains the K largest elements,
  // but possibly not in sorted order, so we sort them for fun
  std::sort(largest.begin(), largest.end(), std::greater<>{});
  return largest[0] == 5 ? 1 : 0;
}

```

### K-smallest Elements in an Array

**Problem**: Given an array of `n` integers, return the `k` smallest elements.

**Solution**: Use a [max-heap]({{< relref heaps >}}) of size `k` to keep track of
the `k` smallest elements. As we iterate through the array, if an element is
smaller than the root of the heap (the smallest of the `k` smallest elements),
we replace the root with the current element.

**Why**: The `max-heap` allows us to efficiently maintain the `k` smallest
elements. Whenever we encounter a smaller element, we replace the largest
element in the heap, keeping the time complexity at $O(n log k)$.

```cpp
#include <vector>
#include <algorithm>
#include <iostream>

[[nodiscard]] std::vector<int> kSmallestElements(const std::vector<int>& nums,
                                                std::size_t k) {
  const auto size = nums.size();
  // Initial heap of size K
  std::vector<int> maxHeap(nums.begin(), nums.begin() + k);
  // Create a max-heap
  std::make_heap(maxHeap.begin(), maxHeap.end());

  for (std::size_t i = k; i < size; ++i) {
    if (nums[i] < maxHeap.front()) {
      // Remove the largest element (moves it to the end)
      std::pop_heap(maxHeap.begin(), maxHeap.end());
      // Replace with the new smaller element
      maxHeap.back() = nums[i];
      // Restore heap property
      std::push_heap(maxHeap.begin(), maxHeap.end());
    }
  }

  // The heap now contains the K smallest elements
  return maxHeap;
}

int main() {
  auto smallest = kSmallestElements({3, 2, 1, 5, 6, 4}, 2);
  // Now the smallest contains the K smallest elements,
  // but possibly not in sorted order, so we sort them for fun
  std::sort(smallest.begin(), smallest.end(), std::less<>{});
  return smallest[1] == 2 ? 1 : 0;
}
```

### K Most Frequent Elements

**Problem**: Given a non-empty array of integers, return the `k` most frequent
elements.

**Solution**: Use a `min-heap` to store the `k` most frequent elements.
First count the frequency of each element, then use the heap to keep
track of the `k` elements with the highest frequency.

**Why**: The min-heap stores the elements with the highest frequencies while
discarding elements with lower frequencies once the heap size exceeds K.
The time complexity of this approach is $O(n log k)$.

```cpp
#include <vector>
#include <unordered_map>
#include <algorithm>

[[nodiscard]] std::vector<int>
kMostFrequentELements(const std::vector<int>& nums, std::size_t k) {
  std::unordered_map<int, int> frequencyMap;
  for(auto num : nums) {
    frequencyMap[num]++;
  }

  std::vector<std::pair<int, int>> minHeap;
  minHeap.reserve(k+1);

  for (const auto& [num, freq] : frequencyMap) {
    minHeap.push_back({freq, num});
    std::push_heap(minHeap.begin(), minHeap.end(), std::greater<>{});

    if(minHeap.size() > k) {
      // restore the Heap property
      std::pop_heap(minHeap.begin(), minHeap.end(), std::greater<>{});
      // remove the last element
      minHeap.pop_back();
    }
  }

  std::vector<int> result;
  result.reserve(k);
  for (const auto& [freq, num] : minHeap) {
    result.push_back(num);
  }

  return result;
}

int main() {
  auto freq = kMostFrequentELements({1, 1, 1, 2, 2, 3}, 2);
  // Now the freq contains the K most frequent elements,
  // but possibly not in sorted order, so we sort them for fun
  // not on frequency but value btw
  std::sort(freq.begin(), freq.end(), std::greater<>{});
  return freq[0] == 2 ? 1 : 0;
}
```

### K Closest Points to the Origin

**Problem**: Given a list of points on a 2D plane, find the `k` closest points
to the origin `(0,0)`.

**Solution**: Use a `max-heap` to keep track of the `k` closest points.
The can use the square-distance from a point $(x,y)$ to the origin.
By comparing the square-distances here we're really just creating a max-heap.

```cpp
#include <vector>
#include <algorithm>
#include <iostream>

struct Point {
  int x = 0;
  int y = 0;
  [[nodiscard]] bool operator<(const Point& Other) const {
    const auto distToOrigin = (x*x) + (y*y);
    const auto otherDistToOrigin = (Other.x*Other.x) + (Other.y*Other.y);
    return distToOrigin < otherDistToOrigin;
  }
};

[[nodiscard]] std::vector<Point>
kClosestPointsToOrigin(const std::vector<Point>& points, std::size_t k) {
  const auto size = points.size();
  std::vector<Point> pointHeap(points.begin(), points.begin() + k);
  std::make_heap(pointHeap.begin(), pointHeap.end());

  for (std::size_t i = k; i < size; ++i) {
    if (points[i] < pointHeap.front()) {
      // Remove the largest element (moves it to the end)
      std::pop_heap(pointHeap.begin(), pointHeap.end());
      // Replace with the new smaller element
      pointHeap.back() = points[i];
      // Restore heap property
      std::push_heap(pointHeap.begin(), pointHeap.end());
    }
  }

  return pointHeap;
}

int main() {
  auto points = kClosestPointsToOrigin({{-1,1}, {0, 1}, {3, 5}, {0, -1}}, 2);
  return points[0].x == 0 ? 1 : 0;
}
```
