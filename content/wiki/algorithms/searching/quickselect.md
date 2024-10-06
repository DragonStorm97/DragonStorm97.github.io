+++
title = 'Quickselect'
date = 2024-10-06T15:39:20+02:00
draft = false
math = true
authors = ["Johnathan Jacobs"]
summary = "Hoare's Selection Algorithm to find the kth order statistic in an unordered list."
tags = ["quick-sort", "quickselect", "search", "traverse", "selecting-k-elements"]
+++

The Quickselect algorithm is a modified version of
[Quicksort]({{< relref "../sorting/quick-sort.md" >}}) that efficiently finds the
$Kth$ largest (or smallest) element in an unordered list. Instead of fully sorting
the array like Quicksort, Quickselect partitions the array around a pivot and
recursively narrows down to the desired $Kth$ element, which results in an average
time complexity of $O(n)$.

Quickselect operates in-place (using no additional space).

Has $O(n^2)$ worst-case time complexity, this is mitigated by selecting
a good pivot.

## Example: Selecting the K-Largest elements

**Problem**: Find the K largest elements in an unsorted array.

**Solution**:

- Partition the array using a pivot. This rearranges the array by selecting a
  pivot and moving all elements larger than the pivot to the left and smaller
  ones to the right.
- Recursively select the partition that contains the $Kth$ element.
- Repeat until the $Kth$ element us found.

**Why**:

- Efficient for $Kth$ element problems: Instead of sorting the entire array,
  Quickselect focuses on partitioning the array to isolate the $Kth$ element,
  leading to a better average time complexity.
- Space efficient: It works in-place, so it doesn't require additional space
  (except for recursion stack).

```cpp
#include <vector>
#include <algorithm>

[[nodiscard]] int partition(std::vector<int>& nums, int left, int right) {
  int pivotIndex = left + rand() % (right - left + 1);  // Randomly select a pivot
  int pivotValue = nums[pivotIndex];
  std::swap(nums[pivotIndex], nums[right]);  // Move pivot to end
  int storeIndex = left;

  for (int i = left; i < right; ++i) {
    // If we are looking for the K largest, pivot on ">"
    if (nums[i] > pivotValue) {
      std::swap(nums[i], nums[storeIndex]);
      storeIndex++;
    }
  }
  // Move pivot to its correct place
  std::swap(nums[storeIndex], nums[right]);
  return storeIndex;
}

[[nodiscard]] int quickselect(std::vector<int>& nums,
                              int left, int right, int k) {
  // If the list contains only one element
  if (left == right) return nums[left];

  int pivotIndex = partition(nums, left, right);

  // The pivot is in its final sorted position
  if (k == pivotIndex) {
    return nums[k];
  } else if (k < pivotIndex) {
    return quickselect(nums, left, pivotIndex - 1, k);
  } else {
    return quickselect(nums, pivotIndex + 1, right, k);
  }
}

[[nodisrcard]] std::vector<int>
kLargestElementsQuickselect(std::vector<int>& nums, int k) {
  int n = nums.size();
  // Use quickselect to find the Kth largest element (0-indexed)
  quickselect(nums, 0, n - 1, k - 1);

  // Now the first k elements of nums are the largest k elements
  // (in no specific order).
  std::vector<int> result(nums.begin(), nums.begin() + k);
  // Sort to return them in descending order
  std::sort(result.begin(), result.end(), std::greater<>());
  return result;
}

int main() {
  std::vector<int> nums = {3, 2, 1, 5, 6, 4};
  int k = 2;
  auto result = kLargestElementsQuickselect(nums, k);
  return result[0] == 6 ? 1 : 0;
}

```
