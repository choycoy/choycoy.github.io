---
title: "Algorithm: Implement Queue using Stacks "
tags:
  - Algorithm
  - Queue
  - Stack
  - Amortised Analysis
---
💡 This post introduces how to solve the leetcode problem Q. 232: Implement Queue using Stacks.
{: .notice--warning}
Implement a first in first out `(FIFO) queue` using only `two stacks`. The implemented queue should support all the `functions` of a normal `queue` (`push`, `peek`, `pop`, and `empty`).
### Implement the Myqueue class
- `void push(int x)` pushes element `x` to the `back` of the `queue`.
- `int pop()` removes the element from the `front` the queue and returns it.
- `int peek()` returns the element at the `front` of the `queue`.
- `boolean empty()` returns `true` if the queue is empty, `false` otherwise.

### Notes
- You must `only` standard operations of a `stack`, which means only `push to top`, `peek/pop from top`, `size` and `is empty` operations are valid.
- Depending on your language, the `stack` may be not supported natively. You may simulate a stack using a `list` or `deque`(double-ended queue) as long as you use only a `stack`s standard operations.

![example](https://user-images.githubusercontent.com/40441643/208372989-f55bd69c-e08f-412f-bae2-d472fe6218b3.PNG)

### Solution
`Queue` is **FIFO**(First In - First Out) data structure, in which the elements are inserted one side - `rear` and removed from the other - `front`. The most intuitive way to implement it is with `Linked lists`, but this post will introduce another approach using `stacks`. `Stack` is **LIFO**(Last In - First Out) data structure, in which elements are added and removed from the same end, called `top`. To satisfy **FIFO** property of a `queue` we need to keep `two stacks`. They serve to **reverse arrival order of the elements** and one of them store the `queue` elements in their final order.

### # Approach 1. (Two Stacks) Push - O(n) per operation, Pop - O(1) per operation
## 1. Push
A `queue` is **FIFO** but a `stack` is **LIFO**. This means the **newest element**5 must be `pushed` to the `bottom of the stack`. To do so we first transfer all `s1` elements to auxiliary stack `s2`. Then the **newly arrived element** is `pushed` on top of `s2` and all its elements are `popped` and `pushed` to `s1`.
<br>

![diagram](https://user-images.githubusercontent.com/40441643/208376540-ef06d443-b2b1-4225-86d1-4e89dcfce976.PNG)
<br>

```
private int front;
private Stack<Integer> s1 = new Stack<>();
private Stack<Integer> s2 = new Stack<>();

public void push(int x) {
  if(s1.empty())
    front = x;
  while(!s1.isEmpty())
    s2.push(s1.pop());
  s2.push(x);
  while(!s2.isEmpty())
    s1.push(s2.pop());
}
```
- Complexity Analysis
1. Time Complexity: O(n)
<br>
Each element, with the exception of newly arrived, is **pushed and popped twice**. The last inserted element is popped and pushed once. Therefore this gives `4n + 2` operations where `n` is the queue size. The `push` and `pop` operations have `O(1)` time complexity.
2. Space Complexity: O(n)
<br>
We need additional memory to store the `queue` elements.

## 2. Pop
The algorithm `pops` an element from the stack `s1`, because `s1` stores always on its `top` the **first inserted element** in the `queue`. The **front element** of the `queue` is kept as `front`.
<br>
<br>
![pop](https://user-images.githubusercontent.com/40441643/208381161-a74e9d90-6d28-4de9-9b56-09396529a431.PNG)
<br>
```
public int pop() {
  int res = s1.pop();
  if(!s1.empty())
    front = s1.peek();
  return res;
}
```
- Complexity Analysis
1. Time Complexity: O(1)
2. Space Complexity: O(1)

## 3. Empty
`Stack` `s1` contains all stack elements, so the algorithm checks `s1` size to return if the queue is `empty`.
```
public boolean empty() {
  return s1.isEmpty();
}
```
- Complexity Analysis
1. Time Complexity: O(1)
2. Space Complexity: O(1)

## 4. Peek
The `front` element is kept in constant memory and is modified when we `push` or `pop` an element.
```
// Get the front element.
public int peek() {
  return front;
}
```

1. Time Complexity: O(1)
<br>
The `front` element has been calculated in advance and only returned in `peek` operation.
2. Space Complexity: O(1)

### # Approach 2. (Two Stacks) Push - O(1) per operation, Pop- Amortised O(1) per operation
## 1. Push
The **newly arrived element** is always added on `top` of `stack s1` and the **first element** is kept as `front` `queue` element.
<br>
<br>
![push2](https://user-images.githubusercontent.com/40441643/208383803-bcc33267-4e94-4724-ac44-bd07630626b8.PNG)
```
private int front;
private Stack<Integer> s1 = new Stack<>();
private Stack<Integer> s2 = new Stack<>();

// Push element x to the back of queue.
public void push(int x) {
  if(s1.empty())
    front = x;
  s1.push(x);
}
```

- Complexity Analysis
1. Time Complexity: O(1)
<br>
Appending an element to a `stack` is an O(1) operation.
2. Space Complexity: O(n)
<br>
We need additional memory to store the `queue` elements.

## 2. Pop
we have to `remove` element in front of the `queue`. This is the **first inserted element** in the `stack s1` and it is positioned at the **bottom of the stack** because of `stack` 's **LIFO (Last In - First Out)** policy. To remove the **bottom element** from `s1`, we have to `pop` all elements from `s1` and to `push` them on to an additional `stack s2`, which helps us to store the elements of `s1` in **reversed order**. This way the **bottom element** of `s1` will be positioned on **top** of `s2` and we can simply `pop` it from stack `s2`. Once `s2` is **empty**, the algorithm transfer data from `s1` to `s2` again.
<br>
<br>
![pop2](https://user-images.githubusercontent.com/40441643/208579730-a5699cc3-249b-4e7a-9a6c-fc64b05b9542.PNG)
```
public int pop() {
  if(s2.isEmpty()) {
    while(!s1.isEmpty())
        s2.push(s1.pop());
  }
  return s2.pop();
}
```
- Complexity Analysis
1. Time Complexity: `Amortised O(1)`, `Worst-case O(n)`
<br>
In the **worst case scenario** when `stack s2` is `empty`, the algorithm **pops** `n elements` from `stack s1` and **pushes** `n elements` to `s2`, where `n` is the `queue size`. This gives `2n` operations, which is `O(n)`. But when stack `s2` is **not empty** the algorithm has `O(1) time complexity`.
2. Space Complexity: O(1)

## Amortised Analysis
`Amortised analysis` gives the `average performance (over time)` of each operation in the **worst case**. The basic idea is that a **worst case operation** can alter the state in such a way that ***the worst case cannot occur again for a long time*** thus `amortising its cost`.
<br>
<br>
Consider this example where we start with an `empty queue` with the following sequence of operations applied:
```
push1, push2,..., pushn, pop1, pop2,...,popn
```
The **worst case time complexity** of a `single pop` operation is `O(n)`. Since we have `n pop` operations, using the `worst-case` per operation analysis gives us a total of `O(n^2)` time.
<br>
<br>
However, in a sequence of operations the `worst case` does **not occur** often in each operation - some operations may be `cheap`, some may be `expensive`. Therefore, a traditional `worst-case` per operation analysis can ***give overly pessimistic bound***. For example, in a dynamic array only some inserts take a **linear time**, through others - **a constant time**.
<br>
<br>
In the example above, the number of times `pop` operations can be called is **limited** by the number of `push` operations before it. Although a `single pop` operation could be **expensive** only once per `n` times (queue size), when `s2` is `empty` and there is a need for **data transfer** between `s1` and `s2`. Hence the total time complexity of the sequence is : `n` (for `push` operation) + `2*n` (for `first pop` operation) + `n-1` (for `pop` operations) which is `O(2*n)`. This gives `O(2n/2n) = O(1)` **average time** per operation.

## 3. Empty
Both stacks `s1` and `s2` contain all `stack` elements, so the algorithm checks `s1` and `s2` size to return if the queue is `empty`.
```
// Return whether the queue is empty.
public boolean empty() {
  return s1.isEmpty() && s2.isEmpty();
}
```

- Complexity Analysis
1. Time Complexity: O(1)
2. Space Complexity: O(1)

## 4. peek
The `front` element is kept in `constant memory` and is modified when we `push` an element. When `s2` is `not empty`, `front element` is positioned on the `top` of `s2`.
```
// Get the front element.
public int peek() {
  if(!s2.isEmpty()){
    return s2.peek();
  }
  return front;
}
```
- Complexity Analysis
1. Time Complexity: O(1)
<br>
The `front` element was either **previously calculated** or **returned** as a `top` element of `stack s2`.
2. Space Complexity: O(1)

### # Simple Solution, short and amortised O(1)
```
class MyQueue {
  Stack<Integer> input = new Stack();
  Stack<Integer> output = new Stack();

  public void push(int x) {
    input.push(x);
  }

  public void pop() {
    peek();
    output.pop();
  }

  public int peek() {
    if(output.empty())
      while(!input.empty())
          output.push(input.pop());
    return output.peek();
  }

  public boolean empty() {
    return input.empty() && output.empty();
  }

}
```
