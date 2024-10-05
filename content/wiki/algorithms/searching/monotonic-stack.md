+++
title = 'Monotonic Stack'
date = 2024-10-05T20:32:31+02:00
draft = false
math = true
authors = ["Johnathan Jacobs"]
summary = "Technique used to for finding the next greater, or smaller element in an array."
tags = ["search", "traverse", "dynamic-programming", "stack", "monotonic-stack", "smallest-element", "largest-element"]
+++

Technique used to for finding the next greater, or smaller element in an array
while maintaining a linear time complexity.
The Idea is to maintain the stack in either an increasing or decreasing (monotonic)
order, which allows us to solve problems in linear time that would otherwise
require nested loops.

## Use Cases

### Next Greater/Smaller Element in an Array

**Problem**: Given an array of integers, find the next greater/smaller element
for each element in the array. The next greater element for an element `x` is the
first greater element to the right of `x` in the array. If no such element exists,
return `-1`.

**Solution**: Use a stack to keep track of the elements for which we haven't found
a greater element yet. Traverse the array from right to left so that we can easily
check if the current element is smaller than elements in the stack.

**Why**: The stack ensures that we only traverse each element once, achieving $O(n)$
time complexity; instead of the brute-force approach of checking every pair of
elements which would be $O(n^2)$.

```cpp
#include <vector>
#include <stack>

[[nodiscard]] std::vector<int> nextGreaterElement(const std::vector<int>& nums) {
  const auto size = nums.size();
  std::vector<int> result(size, -1);
  std::stack<int> s;// stack to maintain the indices of elements

  for(int i = size - 1; i >= 0; --i) {
    // pop all smaller elements
    // change the "<=" here to ">=" for "nextSmallerElement"
    while (!s.empty() && nums[s.top()] <= nums[i]) {
      s.pop();
    }
    if(!s.empty()) {
      // top of the stack is the next greater element
      result[i] = nums[s.top()];
    }
    s.push(i);
  }
  return result;
}

int main() {
  return nextGreaterElement({2, 1, 2, 4, 2})[0] == 4 ? 1 : 0;
}
```

### Largest Rectangle in a Histogram

**Problem**: Given an array representing the heights of bars in a histogram,
find the largest rectangle that can be formed by any number of contiguous
bars.

**Solution**: Use a monotonic stack to keep track of the indices of the bars
in increasing order. For each bar, calculate the maximum possible rectangle
size by finding the width between the current bar and the bars in the stack.

**Why**: The stack helps us find the largest rectangle that can be formed using
each bar as the smallest bar. This avoids the need for nested loops, reducing
time complexity to $O(n)$.

```cpp
#include <vector>
#include <stack>

[[nodiscard]] int largestRectangleArea(const std::vector<int>& heights) {
  std::stack<int> s;
  int maxArea = 0;
  int size = heights.size();

  for(auto i = 0; i<=size; ++i) {
    // add bar of height 0 at the end
    const int height = (i == size) ? 0 : heights[i];
    while(!s.empty() && height < heights[s.top()]) {
      const int prevHeight = heights[s.top()];
      s.pop();
      const int width = s.empty() ? i : i - s.top() - 1;
      maxArea = std::max(maxArea, prevHeight * width);
    }
    s.push(i);
  }
  return maxArea;
}

int main() {
  return largestRectangleArea({2, 1, 5, 6, 2, 3}) == 10 ? 1 : 0;
}
```

### Daily Temperatures

**Problem**: Given a list of daily temperatures, return a list such that, for each
day in the input, the answer tells you how many days you would have to wait until
a warmer temperature. If there is no future day for which this is possible, put `0`
instead.

**Solution**: Use a stack to store indices of days with temperatures for which we
haven't found a warmer temperature yet. Traverse the list from right to left and
check if the current temperature is smaller than the temperatures on the stack.

```cpp
#include <vector>
#include <stack>

[[nodiscard]] std::vector<int>
dailyTemperatures(const std::vector<int>& temperatures) {
  const auto size = temperatures.size();
  std::vector<int> result(size, 0);
  std::stack<int> s;

  for (int i = size - 1; i >= 0; --i) {
    while (!s.empty() && temperatures[s.top()] <= temperatures[i]) {
      s.pop();
    }
    if (!s.empty()) {
      result[i] = s.top() - i;
    }
    s.push(i);
  }
  return result;
}

int main() {
  return dailyTemperatures({73, 74, 75, 71, 69, 72, 76, 73})[2] == 4 ? 1 : 0;
}
```

### Trapping Rain Water

**Problem**: Given n non-negative integers representing an elevation map where
the width of each bar is 1, compute how much water it can trap after raining.

**Solution**: Use a stack to keep track of the indices of the bars, and calculate
the water trapped between the bars when we encounter a taller bar.

```cpp
#include <vector>
#include <stack>

[[nodiscard]] int trap(const std::vector<int>& height) {
  const auto size = height.size();
  int totalWater = 0;
  std::stack<int> s;

  for (int i = 0; i < size; ++i) {
    while (!s.empty() && height[i] > height[s.top()]) {
      const int prev = s.top();
      s.pop();
      if (s.empty()) {
        break;
      }
      const int cur = s.top();
      const int distance = i - cur - 1;
      const int boundedHeight = std::min(height[i], height[cur]) - height[prev];
      totalWater += distance * boundedHeight;
    }
    s.push(i);
  }
  return totalWater;
}

int main() {
  return trap({0,1,0,2,1,0,1,3,2,1,2,1}) == 6 ? 1 : 0;
}
```
