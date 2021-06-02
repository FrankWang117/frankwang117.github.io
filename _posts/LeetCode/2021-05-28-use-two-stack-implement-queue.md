---
layout: post
title: LeetCode Offer 09. 用两个栈实现队列
categories: [LeetCode]
description: 用两个栈实现队列
keywords: LeetCode, JavaScript, stack, queue
---

# Offer 09. 用两个栈实现队列

## 题目

用两个栈实现一个队列。队列的声明如下，请实现它的两个函数 appendTail 和 deleteHead ，分别完成在队列尾部插入整数和在队列头部删除整数的功能。(若队列中没有元素，deleteHead  操作返回 `-1` )



示例 1：

```
输入：

["CQueue","appendTail","deleteHead","deleteHead"]
[[],[3],[],[]]
输出：[null,null,3,-1]
```


示例 2：

```
输入：
["CQueue","deleteHead","appendTail","appendTail","deleteHead","deleteHead"]
[[],[],[5],[2],[],[]]
输出：[null,-1,null,null,5,2]
```


提示：

> 1 <= values <= 10000
> 最多会对  appendTail、deleteHead 进行  10000  次调用



## 题解

用先进后出的栈来实现先进先出的队列，主要是通过数组的 `.pop()` 以及 `.push()` 实现两个栈的数据变化，通过两个栈的数据交换，实现队列。

## 解答

```typescript
class CQueue {
  // 通过数组定义两个栈，只能使用
  // .pop() / .push() 
  // 两个方法
    private stack1 = [];
    private stack2 = [];

    constructor() { }

    appendTail(value: number): void {
      // 队列的每次新增数据都直接放如 stack1 栈内
        this.stack1.push(value)
    }

    deleteHead(): number {
      // 当要删除此队列的数据时，要删除的是最早进来的，
      // 最早进来的数据现在是在 stack1 的栈底，
      // 当前最后进来的在 stack1 的栈顶
      // 那我们将 stack1 中的数据反转过来，就可以将 stack 的栈顶（最早进来的数据）
      // 删除掉，实现队列的先进先出
      // 那怎么将 stack1 中的数据反转过来呢？
      // 通过 stack2 做一个过渡：
      /**
       * 将 stack1 的数据 pop() 出来放入 stack2 中
       * stack2.push(stack1.pop())
       */
      // 现在 stack2 中的数据就是 stack1 中的数据的反转，
      // 也就是最早进入 stack1 中的数据现在在 stack2 的栈顶
      // 从而实现队列这样的数据结构
      // 那如果执行 deleteHead 后， stack2 中存有数据，又
      // appendTail 一个数据进入 stack1 中，从 stack2 中
      // 删除数据会出现问题么？ 不会，因为 stack2 中保存的都是
      // 上个 stack1 数据的反转，也就是比当前的 stack1 数据要新
      // 直到 stack2 清空后，才再次将数据从 stack1 -> stack2
        if(this.stack2.length === 0) {
            while(this.stack1.length > 0) {
                const currentItem = this.stack1.pop();
                this.stack2.push(currentItem)
            }  
        }
        return this.stack2.length ? this.stack2.pop(): -1;
    }
}

/**
 * Your CQueue object will be instantiated and called as such:
 * var obj = new CQueue()
 * obj.appendTail(value)
 * var param_2 = obj.deleteHead()
 */
```

## 解析

见代码

## 想法

熟能生巧。唯有多练习。
