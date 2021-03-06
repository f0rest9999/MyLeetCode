#LeetCode总结及复习计划

![image-20201214145325944](https://gitee.com/f0rest9999/images/raw/master/20201214145326.png)

```
12-14留念
```

```markdown
**300道题留念，以后佛系复习了，不能一天做好多道了,9月15开始做的，到11月9号，54天，平均每天5.5道，好像有点多了哈哈，前期比较艰难，基本看题解的比较多，后期自己能做出来一些中等题了，感觉进步肯定是有的，在想如果本科生的时候就开始刷，或许现在也是个算法小生了吧，但是没有后悔药，我也不想吃。
十月份坚持了每天打卡刷那个题，拿了个勋章，就是为了告诉自己其实坚持并不是太难的事儿。
（深绿色的格子也不知道当天经历了什么，啥也没干就刷题了，不然怎么提交了这么多次哈哈）
这段多复习吧，有些题当时掌握的就不好。每天复习一到两个题就可以，二刷找优解，吃透。
每周周末可以做4-5道新的题（最好是中等），保持思维和手感。
准备按以下线路复习：
动态规划（好久没做动态规划的题了，有些忘记了）
二叉树（主要可以练习递归，递归有时候能很快写出来，有时候想半天想不通，把做过的题总结一下）链表（基本也是递归，可见递归的地位）
递归（N后问题 等经典问题，忘记了）
回溯（基本回溯的题，没有自己做出来的，二刷要像学新知识一样）
图（题目并不多，大部分也涉及递归）
贪心（好像做的题也不多）
位运算（有些题的解法比较tricky，自己想不容易想出来，多复习就好，没有特别的难算法）
滑动窗口（题目较少，可以多做点新题，二刷要像学新知识一样）
堆（几个题目，但是感觉挺经典）
二分查找（这个边界有点烦）
并查集（不算是重点，把做过题复习一下就好）
设计题（复习一遍就行，不再上代码做了就）
```

✅:复习时候有思路且正确

❓:思路正确没写出来

❌:思路错误或者没有思路

⚪:简单题

🔵:中等题

🔴:难题

⭐：好题

⚡：第一次做

👀：参考题解的思路，自己写代码

😕：看题解代码，自己才会写，尽量不要这样

*之前已经做过一些二刷了今天先一并整理到这里,所以11-9日的很多，之后便正常做就可以了*

###二叉树

|                             题目                             |    日期    | 状态 |           备注           |
| :----------------------------------------------------------: | :--------: | :--: | :----------------------: |
| [94. 二叉树的中序遍历](https://leetcode-cn.com/problems/binary-tree-inorder-traversal/) | 2020-11-9  |  ❓🔵  |      迭代算法，经典      |
| [95. 不同的二叉搜索树 II](https://leetcode-cn.com/problems/unique-binary-search-trees-ii/) | 2020-11-9  |  ❌🔵  |      回溯，构造树⭐       |
| [98. 验证二叉搜索树](https://leetcode-cn.com/problems/validate-binary-search-tree/) | 2020-11-9  |  ❌🔵  |           递归           |
| [100. 相同的树](https://leetcode-cn.com/problems/same-tree/) | 2020-11-9  |  ✅⚪  |                          |
| [101. 对称二叉树](https://leetcode-cn.com/problems/symmetric-tree/) | 2020-11-9  |  ✅⚪  |                          |
| [102. 二叉树的层序遍历](https://leetcode-cn.com/problems/binary-tree-level-order-traversal/) | 2020-11-9  |  ✅🔵  |                          |
| [104. 二叉树的最大深度](https://leetcode-cn.com/problems/maximum-depth-of-binary-tree/) | 2020-11-9  |  ✅⚪  |                          |
| [105. 从前序与中序遍历序列构造二叉树](https://leetcode-cn.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/) | 2020-11-9  | ❌🔵⭐  |      递归构造，弱项      |
| [106. 从中序与后序遍历序列构造二叉树](https://leetcode-cn.com/problems/construct-binary-tree-from-inorder-and-postorder-traversal/) | 2020-11-9  | ✅🔵⭐  | 有了上个题的基础做出来的 |
| [107. 二叉树的层次遍历 II](https://leetcode-cn.com/problems/binary-tree-level-order-traversal-ii/) | 2020-11-9  |  ✅⚪  |                          |
| [108. 将有序数组转换为二叉搜索树](https://leetcode-cn.com/problems/convert-sorted-array-to-binary-search-tree/) | 2020-11-9  |  ❌🔵  |   递归构造树中最简单的   |
| [110. 平衡二叉树](https://leetcode-cn.com/problems/balanced-binary-tree/) | 2020-11-9  | ❓⚪⭐  |           经典           |
| [111. 二叉树的最小深度](https://leetcode-cn.com/problems/minimum-depth-of-binary-tree/) | 2020-11-9  |  ❓⚪  |                          |
| [112. 路径总和](https://leetcode-cn.com/problems/path-sum/)  | 2020-11-9  |  ✅⚪  |                          |
| [116. 填充每个节点的下一个右侧节点指针](https://leetcode-cn.com/problems/populating-next-right-pointers-in-each-node/) | 2020-11-9  |  ❓🔵  |                          |
| [117. 填充每个节点的下一个右侧节点指针 II](https://leetcode-cn.com/problems/populating-next-right-pointers-in-each-node-ii/) | 2020-11-9  |  ✅🔵  | 有了上个题的基础做出来的 |
| [129. 求根到叶子节点数字之和](https://leetcode-cn.com/problems/sum-root-to-leaf-numbers/) | 2020-11-9  |  ✅🔵  |                          |
| [144. 二叉树的前序遍历](https://leetcode-cn.com/problems/binary-tree-preorder-traversal/) | 2020-11-9  |  ❓🔵  |         迭代算法         |
| [145. 二叉树的后序遍历](https://leetcode-cn.com/problems/binary-tree-postorder-traversal/) | 2020-11-9  |  ❓🔵  |         迭代算法         |
| [199. 二叉树的右视图](https://leetcode-cn.com/problems/binary-tree-right-side-view/) | 2020-11-9  |  ✅🔵  |                          |
| [222. 完全二叉树的节点个数](https://leetcode-cn.com/problems/count-complete-tree-nodes/) | 2020-11-9  |  ✅🔵  |                          |
| [226. 翻转二叉树](https://leetcode-cn.com/problems/invert-binary-tree/) | 2020-11-9  |  ✅⚪  |                          |
| [230. 二叉搜索树中第K小的元素](https://leetcode-cn.com/problems/kth-smallest-element-in-a-bst/) | 2020-11-9  |  ✅🔵  |                          |
| [236. 二叉树的最近公共祖先](https://leetcode-cn.com/problems/lowest-common-ancestor-of-a-binary-tree/) | 2020-11-9  |  ❌🔵  |           经典           |
| [257. 二叉树的所有路径](https://leetcode-cn.com/problems/binary-tree-paths/) | 2020-11-9  |  ✅⚪  |                          |
| [337. 打家劫舍 III](https://leetcode-cn.com/problems/house-robber-iii/) | 2020-11-11 | ❌🔵⭐  |          (1)⚡👀           |
| [404. 左叶子之和](https://leetcode-cn.com/problems/sum-of-left-leaves/) | 2020-11-20 |  ✅⚪  |                          |
| [113. 路径总和 II](https://leetcode-cn.com/problems/path-sum-ii/) | 2020-11-24 |  ❌🔵  |            ⚡             |
| [897. 递增顺序查找树](https://leetcode-cn.com/problems/increasing-order-search-tree/) | 2020-11-25 |  ✅⚪  |            ⚡             |
| [501. 二叉搜索树中的众数](https://leetcode-cn.com/problems/find-mode-in-binary-search-tree/) | 2020-11-25 |  ✅⚪  |            ⚡             |
| [109. 有序链表转换二叉搜索树](https://leetcode-cn.com/problems/convert-sorted-list-to-binary-search-tree/) | 2020-12-2  |  ❓🔵  |          ⚡构造           |
| [96. 不同的二叉搜索树](https://leetcode-cn.com/problems/unique-binary-search-trees/) | 2020-12-3  |  ✅🔵  |            ⚡             |
| [114. 二叉树展开为链表](https://leetcode-cn.com/problems/flatten-binary-tree-to-linked-list/) | 2020-12-7  | ❌🔵⭐  |          ⚡递归           |
| [235. 二叉搜索树的最近公共祖先](https://leetcode-cn.com/problems/lowest-common-ancestor-of-a-binary-search-tree/) | 2020-12-15 |  ⚪   |           没做           |
|                                                              |            |      |                          |
|                                                              |            |      |                          |
|                                                              |            |      |                          |
|                                                              |            |      |                          |
|                                                              |            |      |                          |
|                                                              |            |      |                          |
|                                                              |            |      |                          |
|                                                              |            |      |                          |
|                                                              |            |      |                          |
|                                                              |            |      |                          |
|                                                              |            |      |                          |
|                                                              |            |      |                          |
|                                                              |            |      |                          |
|                                                              |            |      |                          |
|                                                              |            |      |                          |

### 动态规划

|                             题目                             |    日期    | 状态 |         备注         |
| :----------------------------------------------------------: | :--------: | :--: | :------------------: |
| [53. 最大子序和](https://leetcode-cn.com/problems/maximum-subarray/) | 2020-11-10 |  ✅⚪  |      可压缩空间      |
| [70. 爬楼梯](https://leetcode-cn.com/problems/climbing-stairs/) | 2020-11-10 |  ✅⚪  |      可压缩空间      |
| [121. 买卖股票的最佳时机](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock/) | 2020-11-10 |  ❌⚪  |      可压缩空间      |
| [198. 打家劫舍](https://leetcode-cn.com/problems/house-robber/) | 2020-11-11 |  ❌⚪  |        不熟悉        |
| [213. 打家劫舍 II](https://leetcode-cn.com/problems/house-robber-ii/) | 2020-11-11 |  ✅🔵  |       一次AC⚡        |
| [337. 打家劫舍 III](https://leetcode-cn.com/problems/house-robber-iii/) | 2020-11-11 | ❌🔵⭐  |        (2)⚡👀         |
| [152. 乘积最大子数组](https://leetcode-cn.com/problems/maximum-product-subarray/) | 2020-11-11 | ❌🔵⭐  |          ⚡👀          |
| [5. 最长回文子串](https://leetcode-cn.com/problems/longest-palindromic-substring/) | 2020-11-12 | ❌🔵⭐⭐ |   ⚡😕十分详细的题解   |
| [62. 不同路径](https://leetcode-cn.com/problems/unique-paths/) | 2020-11-13 |  ✅🔵  |        一次AC        |
| [63. 不同路径 II](https://leetcode-cn.com/problems/unique-paths-ii/) | 2020-11-13 |  ✅🔵  |        一次AC        |
| [64. 最小路径和](https://leetcode-cn.com/problems/minimum-path-sum/) | 2020-11-13 |  ✅🔵  |       一次AC⚡        |
| [221. 最大正方形](https://leetcode-cn.com/problems/maximal-square/) | 2020-11-14 |  ❌🔵  |          ⚡😕          |
| [1277. 统计全为 1 的正方形子矩阵](https://leetcode-cn.com/problems/count-square-submatrices-with-all-ones/) | 2020-11-14 |  ✅🔵  |          ⚡           |
| [264. 丑数 II](https://leetcode-cn.com/problems/ugly-number-ii/) | 2020-11-15 |  ❓🔵  |        不熟悉        |
| [300. 最长上升子序列](https://leetcode-cn.com/problems/longest-increasing-subsequence/) | 2020-11-16 | ❌🔵⭐⭐ |     经典+可贪心      |
| [343. 整数拆分](https://leetcode-cn.com/problems/integer-break/) | 2020-11-16 |  ✅🔵  |       ==剪绳子       |
| [剑指 Offer 14- I. 剪绳子](https://leetcode-cn.com/problems/jian-sheng-zi-lcof/) | 2020-11-16 |  ✅🔵  |      ==整数拆分      |
| [392. 判断子序列](https://leetcode-cn.com/problems/is-subsequence/) | 2020-11-17 |  ❓⚪  | 暴力好做，DP辅助难想 |
| [368. 最大整除子集](https://leetcode-cn.com/problems/largest-divisible-subset/) | 2020-11-18 | ❌🔵⭐  |          ⚡           |
| [416. 分割等和子集](https://leetcode-cn.com/problems/partition-equal-subset-sum/) | 2020-11-19 | ❓🔵⭐⭐ |   0-1背包的小变形    |
| [523. 连续的子数组和](https://leetcode-cn.com/problems/continuous-subarray-sum/) | 2020-11-19 |  ❌🔵  |       题很一般       |
| [413. 等差数列划分](https://leetcode-cn.com/problems/arithmetic-slices/) | 2020-11-20 |  ✅🔵  |          ⚡           |
| [673. 最长递增子序列的个数](https://leetcode-cn.com/problems/number-of-longest-increasing-subsequence/) | 2020-11-30 | ❓🔵⭐⭐ |          ⚡           |
| [1143. 最长公共子序列](https://leetcode-cn.com/problems/longest-common-subsequence/) | 2020-12-9  | ✅🔵⭐  |       ⚡一次AC        |
| [120. 三角形最小路径和](https://leetcode-cn.com/problems/triangle/) | 2020-12-11 |  ✅🔵  |       ⚡一次AC        |
| [面试题 01.05. 一次编辑](https://leetcode-cn.com/problems/one-away-lcci/) | 2020-12-12 |  ❓🔵  |          ⚡           |
| [91. 解码方法](https://leetcode-cn.com/problems/decode-ways/) | 2020-12-16 |  ❌🔵  |          ⚡           |
|                                                              |            |      |                      |
|                                                              |            |      |                      |
|                                                              |            |      |                      |
|                                                              |            |      |                      |
|                                                              |            |      |                      |
|                                                              |            |      |                      |
|                                                              |            |      |                      |
|                                                              |            |      |                      |
|                                                              |            |      |                      |
|                                                              |            |      |                      |
|                                                              |            |      |                      |
|                                                              |            |      |                      |
|                                                              |            |      |                      |
|                                                              |            |      |                      |
|                                                              |            |      |                      |
|                                                              |            |      |                      |
|                                                              |            |      |                      |
|                                                              |            |      |                      |
|                                                              |            |      |                      |
|                                                              |            |      |                      |
|                                                              |            |      |                      |
|                                                              |            |      |                      |

### 二分查找

|                             题目                             |   日期    | 状态 | 备注 |
| :----------------------------------------------------------: | :-------: | :--: | :--: |
| [ 在排序数组中查找元素的第一个和最后一个位置](https://leetcode-cn.com/problems/find-first-and-last-position-of-element-in-sorted-array) | 2020-12-2 | ❓🔵⭐  |  ⚡   |
|                                                              |           |      |      |
|                                                              |           |      |      |
|                                                              |           |      |      |
|                                                              |           |      |      |
|                                                              |           |      |      |
|                                                              |           |      |      |
|                                                              |           |      |      |

### 链表

|                             题目                             |   日期    | 状态 |     备注      |
| :----------------------------------------------------------: | :-------: | :--: | :-----------: |
| [1171. 从链表中删去总和值为零的连续节点](https://leetcode-cn.com/problems/remove-zero-sum-consecutive-nodes-from-linked-list/) | 2020-12-2 |  ❌🔵  |       ⚡       |
| [61. 旋转链表](https://leetcode-cn.com/problems/rotate-list/) | 2020-12-2 |  ✅🔵  |       ⚡       |
| [206. 反转链表](https://leetcode-cn.com/problems/reverse-linked-list/) | 2020-12-4 | ✅⚪⭐  |               |
| [92. 反转链表 II](https://leetcode-cn.com/problems/reverse-linked-list-ii/) | 2020-12-4 | ❓🔵⭐⭐ | 递归的好题目⚡ |
| [21. 合并两个有序链表](https://leetcode-cn.com/problems/merge-two-sorted-lists/) | 2020-12-5 | ❓⚪⭐  |     递归      |
| [24. 两两交换链表中的节点](https://leetcode-cn.com/problems/swap-nodes-in-pairs/) | 2020-12-5 |  ✅🔵  |  递归一次AC   |
| [82. 删除排序链表中的重复元素 II](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-list-ii/) | 2020-12-5 | ❓🔵⭐  |  递归和迭代   |
| [面试题 02.05. 链表求和](https://leetcode-cn.com/problems/sum-lists-lcci/) | 2020-12-5 | ❌🔵⭐  |       ⚡       |
| [328. 奇偶链表](https://leetcode-cn.com/problems/odd-even-linked-list/) | 2020-12-6 |  ❌🔵  |       ⚡       |
| [445. 两数相加 II](https://leetcode-cn.com/problems/add-two-numbers-ii/) | 2020-12-6 |  ❌🔵  |       ⚡       |
|                                                              |           |      |               |
|                                                              |           |      |               |
|                                                              |           |      |               |
|                                                              |           |      |               |

### 回溯

|                             题目                             |    日期    | 状态  |        备注         |
| :----------------------------------------------------------: | :--------: | :---: | :-----------------: |
| [39. 组合总和](https://leetcode-cn.com/problems/combination-sum/) | 2020-12-8  |  ❌🔵⭐  |          ⚡          |
| [46. 全排列](https://leetcode-cn.com/problems/permutations/) | 2020-12-8  |  ❌🔵⭐  |          ⚡          |
| [47. 全排列 II](https://leetcode-cn.com/problems/permutations-ii/) | 2020-12-8  |  ❓🔵⭐  |          ⚡          |
|  [77. 组合](https://leetcode-cn.com/problems/combinations/)  | 2020-12-8  |  ✅🔵⭐  |         ⚡AC         |
|    [78. 子集](https://leetcode-cn.com/problems/subsets/)     | 2020-12-9  |  ✅🔵⭐  |         AC          |
| [90. 子集 II](https://leetcode-cn.com/problems/subsets-ii/)  | 2020-12-9  |  ✅🔵⭐  |         ⚡AC         |
| [60. 排列序列](https://leetcode-cn.com/problems/permutation-sequence/) | 2020-12-9  |  ✅🔴⭐  | ⚡AC but inefficient |
| [93. 复原IP地址](https://leetcode-cn.com/problems/restore-ip-addresses/) | 2020-12-9  |  ❌🔵⭐  |          ⚡          |
| [200. 岛屿数量](https://leetcode-cn.com/problems/number-of-islands/) | 2020-12-13 |  ✅🔵⭐  |     ⚡Flood Fill     |
| [130. 被围绕的区域](https://leetcode-cn.com/problems/surrounded-regions/) | 2020-12-13 |  ✅🔵⭐  |     ⚡Flood Fill     |
| [79. 单词搜索](https://leetcode-cn.com/problems/word-search/) | 2020-12-13 |  ❓🔵⭐  | ⚡Flood Fill，字符串 |
| [17. 电话号码的字母组合](https://leetcode-cn.com/problems/letter-combinations-of-a-phone-number/) | 2020-12-14 |  ❓🔵   |          ⚡          |
| [22. 括号生成](https://leetcode-cn.com/problems/generate-parentheses/) | 2020-12-14 |  ❌🔵   |          ⚡          |
|   [51. N 皇后](https://leetcode-cn.com/problems/n-queens/)   | 2020-12-14 | ❌🔴⭐⭐⭐ |       N后问题       |
| [37. 解数独](https://leetcode-cn.com/problems/sudoku-solver/) | 2020-12-14 |  ❌🔴⭐  |          ⚡          |
| [40. 组合总和 II](https://leetcode-cn.com/problems/combination-sum-ii/) | 2020-12-16 |  ✅🔵⭐  |          ⚡          |
|                                                              |            |       |                     |
|                                                              |            |       |                     |
|                                                              |            |       |                     |



### 数组

|                             题目                             |    日期    | 状态 |     备注     |
| :----------------------------------------------------------: | :--------: | :--: | :----------: |
| [204. 计数质数](https://leetcode-cn.com/problems/count-primes/) | 2020-12-3  |  ✅⚪  | 厄拉多塞筛法 |
| [56. 合并区间](https://leetcode-cn.com/problems/merge-intervals/) | 2020-12-16 |  ❓🔵  | 排序API练习⚡ |
| [59. 螺旋矩阵 II](https://leetcode-cn.com/problems/spiral-matrix-ii/) | 2020-12-16 |  ✅🔵  |      ⚡       |
|                                                              |            |      |              |
|                                                              |            |      |              |
|                                                              |            |      |              |
|                                                              |            |      |              |
|                                                              |            |      |              |

### SQL

|                             题目                             |   日期    | 状态 | 备注 |
| :----------------------------------------------------------: | :-------: | :--: | :--: |
| [184. 部门工资最高的员工](https://leetcode-cn.com/problems/department-highest-salary/) | 2020-12-5 |  ❌🔵  |  ⚡   |
|                                                              |           |      |      |
|                                                              |           |      |      |
|                                                              |           |      |      |
|                                                              |           |      |      |
|                                                              |           |      |      |
|                                                              |           |      |      |
|                                                              |           |      |      |

###滑动窗口

|                             题目                             | 日期 | 状态 | 备注 |
| :----------------------------------------------------------: | :--: | :--: | :--: |
| [剑指 Offer 57 - II. 和为s的连续正数序列](https://leetcode-cn.com/problems/he-wei-sde-lian-xu-zheng-shu-xu-lie-lcof/) |      |  ⚪   | 没做 |
| [剑指 Offer 59 - I. 滑动窗口的最大值](https://leetcode-cn.com/problems/hua-dong-chuang-kou-de-zui-da-zhi-lcof/) |      |  ⚪   | 没做 |
|                                                              |      |      |      |
|                                                              |      |      |      |
|                                                              |      |      |      |
|                                                              |      |      |      |
|                                                              |      |      |      |
|                                                              |      |      |      |

### 位运算

|                             题目                             | 日期 | 状态 | 备注 |
| :----------------------------------------------------------: | :--: | :--: | :--: |
| [136. 只出现一次的数字](https://leetcode-cn.com/problems/single-number/) |      |  ⚪   | 没做 |
| [137. 只出现一次的数字 II](https://leetcode-cn.com/problems/single-number-ii/) |      |  🔵   | 没做 |
| [169. 多数元素](https://leetcode-cn.com/problems/majority-element/) |      |  ⚪   | 没做 |
| [190. 颠倒二进制位](https://leetcode-cn.com/problems/reverse-bits/) |      |  ⚪   | 没做 |
| [191. 位1的个数](https://leetcode-cn.com/problems/number-of-1-bits/) |      |  ⚪   | 没做 |
| [260. 只出现一次的数字 III](https://leetcode-cn.com/problems/single-number-iii/) |      |  🔵   | 没做 |
| [268. 丢失的数字](https://leetcode-cn.com/problems/missing-number/) |      |  ⚪   | 没做 |
| [342. 4的幂](https://leetcode-cn.com/problems/power-of-four/) |      |  ⚪   | 没做 |
| [389. 找不同](https://leetcode-cn.com/problems/find-the-difference/) |      |  ⚪   | 没做 |
| [461. 汉明距离](https://leetcode-cn.com/problems/hamming-distance/) |      |  ⚪   | 没做 |
| [476. 数字的补数](https://leetcode-cn.com/problems/number-complement/) |      |  ⚪   | 没做 |
| [693. 交替位二进制数](https://leetcode-cn.com/problems/binary-number-with-alternating-bits/) |      |  ⚪   | 没做 |
| [1342. 将数字变成 0 的操作次数](https://leetcode-cn.com/problems/number-of-steps-to-reduce-a-number-to-zero/) |      |  ⚪   | 没做 |
| [剑指 Offer 15. 二进制中1的个数](https://leetcode-cn.com/problems/er-jin-zhi-zhong-1de-ge-shu-lcof/) |      |  ⚪   | 没做 |
| [剑指 Offer 39. 数组中出现次数超过一半的数字](https://leetcode-cn.com/problems/shu-zu-zhong-chu-xian-ci-shu-chao-guo-yi-ban-de-shu-zi-lcof/) |      |  ⚪   | 没做 |
| [面试题 17.04. 消失的数字](https://leetcode-cn.com/problems/missing-number-lcci/) |      |  ⚪   | 没做 |

