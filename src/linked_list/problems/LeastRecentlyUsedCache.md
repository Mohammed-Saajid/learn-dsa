
# LRU Cache 


[**LC: 146**](https://leetcode.com/problems/lru-cache/description/)

```cpp
class LRUCache {

private:
    int capacity_;
    list<pair<int,int>> lst; // [(key,val)]
    using ListIterator = list<pair<int,int>>::iterator;
    unordered_map<int,ListIterator> cache; // {(int,[(key, val)])}
public:
    LRUCache(int capacity) : capacity_(capacity) {}
    
    int get(int key) {
        auto map_iterator = cache.find(key);
        if (map_iterator == cache.end()) return -1;
        lst.splice(lst.begin(),lst, map_iterator->second);
        return map_iterator->second->second; 
    }
    
    void put(int key, int value) {
        auto map_iterator = cache.find(key);
        if (map_iterator != cache.end()) {
            map_iterator->second->second = value;
            lst.splice(lst.begin(),lst,map_iterator->second);
            return;
        }
        lst.emplace_front(key,value);
        if ((int)lst.size() >= capacity_) {
            auto lru = lst.back();
            cache.erase(lru.first);
            lst.pop_back();
        }
        cache[key] = lst.begin();
    }
};

```

### Problem

An **LRU (Least Recently Used) Cache** stores a limited number of key-value pairs. When the cache reaches its capacity and a new key needs to be inserted, the **least recently used** key must be removed. Both `get()` and `put()` should ideally work in **O(1) average time**.

### Core Idea

We need two operations to be efficient: **finding a key quickly** and **maintaining the usage order**. An `unordered_map` provides O(1) average-time lookup, while a `list` maintains the elements in usage order. The list stores `(key, value)` pairs, with the **most recently used element at the front** and the **least recently used element at the back**.

### Data Structures

```cpp
list<pair<int, int>> lst;
using ListIterator = list<pair<int, int>>::iterator;
unordered_map<int, ListIterator> cache;
```

The `list` contains the actual `(key, value)` pairs. The `unordered_map` maps each key to an iterator pointing to its corresponding pair inside the list. Therefore, we can find a key in O(1) average time and directly access its position in the list.

### `get(key)`

First, search for the key in the `unordered_map`. If it doesn't exist, return `-1`. If it exists, the corresponding list node is moved to the front using `splice()`, because accessing the key makes it the **most recently used** element. The value is then returned. `splice()` moves the existing list node without creating a new node, and the iterator remains valid.

```cpp
auto map_iterator = cache.find(key);
if (map_iterator == cache.end()) return -1;

lst.splice(lst.begin(), lst, map_iterator->second);
return map_iterator->second->second;
```

### `put(key, value)`

If the key already exists, update its value and move its list node to the front because it has just been used. If the key is new and the cache is already at capacity, remove the element at the back of the list, which is the **least recently used** element, and erase its key from the map. Finally, insert the new `(key, value)` pair at the front and store its iterator in the map.

```cpp
if (map_iterator != cache.end()) {
    map_iterator->second->second = value;
    lst.splice(lst.begin(), lst, map_iterator->second);
    return;
}

if (lst.size() >= capacity_) {
    auto lru = lst.back();
    cache.erase(lru.first);
    lst.pop_back();
}

lst.emplace_front(key, value);
cache[key] = lst.begin();
```

### Why `list::splice()`?

`splice()` allows an existing list node to be moved to another position in **O(1)** time without copying or recreating it. This is important because every successful `get()` or update needs to move an element to the front.

### Why `emplace_front()`?

`emplace_front(key, value)` constructs the `pair<int, int>` directly at the front of the list. `push_front({key, value})` would also work here, but `emplace_front()` directly constructs the element from the provided arguments.

### Iterator Relationship

There are two different iterators involved conceptually. `map_iterator` is an iterator into the `unordered_map`. Its `second` contains a `ListIterator`, and that `ListIterator` points to the actual `(key, value)` pair in the list. Therefore:

```cpp
map_iterator->second
```

gives the list iterator, while:

```cpp
map_iterator->second->second
```

gives the `value` from the `(key, value)` pair.

### Complexity

Both `get()` and `put()` take **O(1) average time** because `unordered_map` lookup is O(1) average, list insertion/removal is O(1), and `splice()` is O(1). The cache uses **O(capacity)** space.
