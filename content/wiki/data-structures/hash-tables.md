+++
title = 'Hash Tables'
date = 2024-09-17T11:47:23+02:00
draft = false
math = true
tags = ["data-structure", "hash-table", "hash", "hash-map", "memoization", "tree"]
authors = ["Johnathan Jacobs"]
+++

A Hash Table (or Hash Map), is a data structure that sores key-value pairs.
It uses a **hash function** to compute an index (bucket) into an array of buckets,
from which the desired value can be found. The keys are unique, and the hash
table handles collisions (two keys hashing to the same index) using methods
such as **chaining** or **open addressing**.

So really, a hash-table is a tree with nodes being bucket hash values,
and their values being a list of the values in them. Where the list would
only contain the searched-for value if the **hash function** is unique for each node.

## Use Cases

Hash tables are ideal for efficient **insertion**, **deletion**, and **lookup** operations.

- **Dictionary/Map**: key-value pairs for fast data access/retrieval in constant-time.
- **Caching/Memoization**: Store recently accessed data for faster retrieval.
- **Counting Frequencies**: Count occurrences of elements
  (really just a dictionary/map example).

## Complexity

- **Insert**: $O(1)$ average, O(n)$ worst-case (if rehashing).
- **Remove**: $O(1)$ average, O(n)$ worst-case.
- **Get**: $O(1)$ average, O(n)$ worst-case.
- **Space Complexity**: $O(n)$, where $n$ is the number of key-value pairs.

## Implementation

### Language Standard Implementations

- C++: `std::unordered_map`
- Go: `map`

### C++ Implementation (Using Separate Chaining for Collision Handling)

```cpp
#include <cassert>
#include <vector>
#include <forward_list>
#include <stdexcept>
#include <functional>

template <typename Key, typename Value>
class HashTable {
private:
  static constexpr std::size_t DEFAULT_CAPACITY = 16;
  static constexpr float LOAD_FACTOR = 0.75;

  struct Bucket {
    Key key;
    Value value;
  };

  std::vector<std::forward_list<Bucket>> table;
  std::size_t numElements = 0;

  std::size_t hash(const Key& key) const {
    return std::hash<Key>{}(key) % table.size();
  }

  void rehash() {
    std::vector<std::forward_list<Bucket>> newTable(table.size() * 2);
    for (const auto& chain : table) {
      // Separate chaining: Search through the linked list at table[idx].
      for (const auto& bucket : chain) {
        auto idx = std::hash<Key>{}(bucket.key) % newTable.size();
        newTable[idx].emplace_front(bucket.key, bucket.value);
      }
    }
    table = std::move(newTable);
  }

public:
  explicit HashTable(std::size_t capacity = DEFAULT_CAPACITY)
    : table(capacity) {}

  // Insert: O(1) average time, O(n) worst-case due to rehashing.
  void insert(const Key& key, const Value& value) {
    if (numElements > table.size() * LOAD_FACTOR) {
      rehash();
    }
    auto idx = hash(key);
    // Separate chaining: Search through the linked list at table[idx].
    for (auto& bucket : table[idx]) {
      if (bucket.key == key) {
        bucket.value = value; // Update value if key exists
        return;
      }
    }
    table[idx].emplace_front(Bucket{key, value});
    ++numElements;
  }

  // Remove: O(1) average time, O(n) worst-case.
  void remove(const Key& key) {
    auto idx = hash(key);
    auto& chain = table[idx];
    chain.remove_if([&](const Bucket& bucket) {
      return bucket.key == key;
    });
    --numElements;
  }

  // Get: O(1) average time, O(n) worst-case.
  [[nodiscard]] const Value& get(const Key& key) const {
    auto idx = hash(key);
    for (const auto& bucket : table[idx]) {
      if (bucket.key == key) {
        return bucket.value;
      }
    }
    throw std::out_of_range("Key not found.");
  }

  // Contains: O(1) average time, O(n) worst-case.
  [[nodiscard]] bool contains(const Key& key) const {
    auto idx = hash(key);
    for (const auto& bucket : table[idx]) {
      if (bucket.key == key) {
        return true;
      }
    }
    return false;
  }

  // Size: O(1), returns the number of elements in the hash table.
  [[nodiscard]] std::size_t size() const noexcept {
    return numElements;
  }
};

int main() {
  HashTable<std::string, int> t;
  t.insert("hello", 1);
  t.insert("world", 2);
  assert(t.contains("hello"));
  assert(t.contains("world"));
  assert(t.get("world") == 2);
  assert(!t.contains("nothing"));
  t.remove("world");
  assert(t.get("hello") == 1);
}
```
