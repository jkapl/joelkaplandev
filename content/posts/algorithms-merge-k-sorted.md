+++ 
date = "2021-07-01" 
title = "Merge K Sorted Lists: Min Heap Solution" 
slug = "merge-k-sorted" 
tags = ["algorithms", "data-structures", "binary-heap"] 
categories = ["Algorithms"] 
series = [] 
+++

```
var mergeKLists = function(lists) {
  // create a min heap with value of first element of each list
        // bubble down from the last element to heapify
  // top of the heap to result list
  // take next element from that list, add it to top of heap. if it's the end of that list, value to add is infinity
  // sink down
  // repeat 2, 4 until top of heap value is infinity
  // nlog(k) - n elements to look at, log(k) sink down operations
    
    // children: L, R: 2*n+1, 2*n+2 
    // parent: Math.floor((n-1)/2)
  
    class Node {
        constructor(val, next) {
            this.val = val;
            this.next = next;
        }
    }
    
    if (lists.length == 0) return null
 
    let heap = [];
    for (let i = 0; i < lists.length; i++) {
        if (lists[i] != null) heap.push(lists[i]);
    }
    if (heap.length == 0) return null
    heapify(heap)

    let finalList = new Node(0, null);
    let head = finalList;
    while(true) {
        let temp = heap[0];
        let tempFinalList = finalList;
        finalList.next = temp;
        finalList = tempFinalList.next;
        heap[0] = heap[0].next;
        if (heap[0] == null) {
            heap[0] = new Node(Number.MAX_SAFE_INTEGER, null)
        }
        bubbleDown(heap, 0)
        if (heap[0].val == Number.MAX_SAFE_INTEGER) break;
    }


    return head.next;
    
    function heapify(heap) {
        // bubble down from the bottom!!!!
        for (let i = heap.length - 1; i >= 0; i--) {
            bubbleDown(heap, i)
        }
    }
    
    function bubbleDown(heap, index) {
        let parent = index
        let childLeft = index * 2 + 1
        let childRight = index * 2 + 2
        while(true) {
            if (heap[childLeft] && heap[childRight]) {
                if (heap[childLeft].val < heap[parent].val && heap[childLeft].val < heap[childRight].val) {
                    swap(heap, childLeft, parent)
                    parent = childLeft;
                    childLeft = parent * 2 + 1
                    childRight = parent * 2 + 2
                } else if (heap[childRight].val < heap[parent].val) {
                    swap(heap, childRight, parent)
                    parent = childRight;
                    childLeft = parent * 2 + 1
                    childRight = parent * 2 + 2
                } else {
                    break;
                }
            } else {
                if (heap[childLeft] && heap[childLeft].val < heap[parent].val) {
                    swap(heap, childLeft, parent)
                    parent = childLeft;
                    childLeft = parent * 2 + 1
                    childRight = parent * 2 + 2
                } else if (heap[childRight] && heap[childRight].val < heap[parent].val) {
                    swap(heap, childRight, parent)
                    parent = childRight
                    childLeft = parent * 2 + 1
                    childRight = parent * 2 + 2
                } else {
                    break;
                }
            }
        }
    }

    function swap(heap, idxA, idxB) {
        let temp = heap[idxA];
        heap[idxA] = heap[idxB];
        heap[idxB] = temp;
    }
};
```