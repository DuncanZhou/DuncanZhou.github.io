---
title: python构建小顶堆
categories: Note
---
近日实验中需要用到小顶堆,记录下来,便于日后参考.
```
import heapq
# 定义一个小顶堆
class MinHeap(object):

    # 允许传入tuple,按照第二个元素比较
    def __init__(self, initial=None, key=lambda x:x[1]):
        self.key = key
        if initial:
            self._data = [(key(item), item) for item in initial]
            heapq.heapify(self._data)
        else:
            self._data = []

    def push(self, item):
        heapq.heappush(self._data, (self.key(item), item))

    def pop(self):
        return heapq.heappop(self._data)[1]

    def size(self):
        return len(self._data)
```
