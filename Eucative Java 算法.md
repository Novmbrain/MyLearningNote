# Eucative Java 算法

## Paradigms模式

### Brute Force暴力算法

暴力算法通常是第一个想到了，它能保证我们至少有一种解法

## Greedy algorithms贪心算法

贪心算法的适用条件：

- 全局最优解可以通过不断迭代局部最优解实现
- 问题的最优解包含其子问题的最优解

## Divide and Conquer分而治之

二叉树问题是典型的分而治之模式的应用。这种模式不断将一个问题分解成他的子问题直到分解到了最小，最原子的子问题。通过解决决子问题并拼接起来完成原始问题

### 709. 转换成小写字母

```java
实现函数 ToLowerCase()，该函数接收一个字符串参数 str，并将该字符串中的大写字母转换成小写字母，之后返回新的字符串。
示例 1：

输入: "Hello"
输出: "hello"
示例 2：

输入: "here"
输出: "here"
示例 3：

输入: "LOVELY"
输出: "lovely"

```





# 左程云算法

## introduction

算法分类

- 明确知道怎么算的流程
- 明确知道怎么尝试的流程

```java
//等概率返回1~N，任何一个数
public static int random(int N){
    //Math.random() [0, N] double
    //Math.random()*N [0, N] double
    //(int) (Math.random() * N) [0, N-1] int 等概率得到一个
    //(int) (Math.random() * N) + 1 [1, N] 等概率返回一个
    return (int) (Math.random() * N) + 1;
}
```

数据结构的基本实现

- 内存里的紧密实现
- 内存里的跳转结构

通过各式各样的算法，能够对基本数据结构的操作（增删改查等等）优化

在数据结构上玩逻辑 （数据结构+算法）== 程序

## 二叉树的基本算法

### 递归序

递归遍历的本质是：递归序？？什么是递归序

以下函数的递归序是：每个head都会回到函数三次

```java
public static void f(Node head){
    if(head == null) return;
    //1
    f(head.left);
    //2
    f(head.right);
    //3
}
```

先序中序后序，其实只是递归序加工的结果

- 前序：head第一次到达f进行打印
- 中序：head第二次到达f进行打印
- 后序：head第三次到达f进行打印

任何一个节点都能去左树逛一圈收集信息。也都能到右树逛一圈收集信息。最后还能在收集完左右节点信息后返回到当前节点。

### 将递归函数改成非递归

任何递归函数都可以改成非递归

二叉树前序遍历的非递归实现：

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */

/**使用非递归的方法实现二叉树的先序遍历
使用栈进行辅助
首先把头结点压入栈
然后弹出栈顶节点，打印栈顶节点。如果右子树非空，入栈，然后如果左子树非空，入栈
循环结束条件为：栈为空

*/
class Solution {
   //如果没有在泛型指明栈内节点类型，在出栈时默认为object 
   //同样使用ArrayList时，如果不定义泛型类型时，泛型类型实际上就是Object
    private Stack<TreeNode> stack = new Stack<>(); 

    public List<Integer> preorderTraversal(TreeNode root) {
            //base case 
            if(root == null) return new ArrayList<Integer>();
            //创建返回的List
            List<Integer> returnList = new ArrayList<>();

            //首先将头结点压入栈
            stack.push(root);
            while(true){
                //弹出栈顶节点
                TreeNode topNode = stack.pop();
                returnList.add(topNode.val);

                if(topNode.right != null) stack.push(topNode.right);
              
                if(topNode.left != null) stack.push(topNode.left);
                
                if(stack.empty()) break; 
            }

            return returnList;  
    }
}
```

在出栈后就打印，如果在压入栈时

- 先压右，再压左。打印顺序是头左右，这是先序
- 如果先压左，再压右。即打印顺序是头右左，然后我们将其逆序输出（也就是出栈后再入另一个栈，然后遍历出栈并打印），就成了左右头，也就是后序
- 中序遍历：1）首先第一个while循环整条左边界依次入栈。2）然后弹出节点，打印，并将头结点设置为当前节点的右子树



思路1： 16进制->十进制？

思路2：16进制直接对应asiic码

38