+++
title = 'Two Pointers'
date = 2024-10-03T21:03:37+02:00
draft = false
math = true
authors = ["Johnathan Jacobs"]
summary = "Algorithm using two pointers or indices into an array to optimize complex problems."
tags = ["search", "traverse", "dynamic-programming", "sliding-window", "two-pointers", "palindrome", "two-sum-problem", "trapping-water"]
+++

Given an array, we take a pointer or index to the start and end, and either move
them towards each other, or away from each other depending on the problem.

Can improve the time complexity of problems from $O(n^2)$ to $O(n)$.

Usually, the two-pointer method requires a sorted array.

## Uses

### Test if String is a Palindrome

Here, we move the pointers/indices towards each other.

```cpp
#include <string>
#include <string_view>

[[nodiscard]] bool isPalindrome(std::string_view str) {
  std::size_t start = 0;
  auto end = str.length() - 1;
  while (start < end) {
    if (str[start] != str[end]) {
      return false;
    }
    ++start;
    --end;
  }
  return true;
}

int main() {
  return isPalindrome("HellolleH") ? 1 : 0;
}
```

### Two-Sum Problem (Sorted Array)

**Problem**: Given a sorted array, find two numbers that sum up to the target value.

**Solution**: Use the two pointers starting at the bounds, sum values at the pointers: if the
sum is less than the target, move the left pointer forward; if the sum is greater,
move the right pointer backward.

```cpp
#include <vector>

[[nodiscard]] std::pair<int, int> twoSumSorted(const std::vector<int>& nums,
                                               int target) {
  std::size_t start = 0;
  auto end = nums.size() - 1;
  while (start < end) {
    const int sum = nums[start] + nums[end];
    if (sum == target){
      return {start, end};
    } else if(sum < target) {
      ++start;
    } else {
      --end;
    }
  }
  return {-1, -1}; // No Solution
}

int main() {
  return twoSumSorted({1, 2, 3, 4, 5, 6}, 10) == std::pair{3, 5} ? 1 : 0;
}
```

### Trapping Rain Water

**Problem**: Given an array of heights, calculate how much water is trapped
between the bars after raining.

**Solution**: Use two pointers (`left` and `right`) starting from both ends. Track
the maximum heights on both sides. If the height on the left is smaller, the
amount of water trapped at the left index is determined by the difference between
the current left height and the left max. Move the pointer inward accordingly.

```cpp
#include <vector>

[[nodiscard]] int trapRainWater(const std::vector<int>& height) {
  std::size_t left = 0;
  auto right = height.size() - 1;
  int leftMax = 0;
  int rightMax = 0;
  int totalWater = 0;

  while (left < right) {
    const auto lHeight = height[left];
    const auto rHeight = height[right];
    if (lHeight < rHeight) {
      if (lHeight >= leftMax) {
        leftMax = lHeight;
      } else {
        totalWater += leftMax - lHeight;
      }
      ++left;
    } else {
      if (rHeight >= rightMax) {
        rightMax = rHeight;
      } else {
        totalWater += rightMax - rHeight;
      }
      --right;
    }
  }
  return totalWater;
}

int main() {
  return trapRainWater({0, 10, 0, 10, 0});
}
```

### Container with Most Water

**Problem**: Find two vertical lines in an array where the container between them
holds the most water. The height of the container is determined by the shorter
of the two lines.

**Solution**: Use two pointers (left and right) starting at the beginning and end
of the array. Calculate the area formed by the lines at these two pointers, then
move the pointer pointing to the shorter line inward to try and find a taller line.

```cpp
#include <vector>

[[nodiscard]] int calculateMaxArea(const std::vector<int>& height) {
  std::size_t left = 0;
  std::size_t right = height.size() - 1;
  int maxArea = 0;
  while (left < right) {
    const auto lHeight = height[left];
    const auto rHeight = height[right];
    const int area = static_cast<int>((right - left) * std::min(lHeight, rHeight));
    maxArea = std::max(area, maxArea);
    if (lHeight < rHeight) {
      ++left;
    } else {
      ++right;
    }
  }
  return maxArea;
}

int main() {
  return calculateMaxArea({0, 10, 0, 5, 0}) == 50 ? 1 : 0;
}
```
