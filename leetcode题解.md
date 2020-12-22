#LeetCode题解

## 动态规划

### 总结

DP记录的数组可以分为一维、二维、多维的，通常是由一维数组、二维数组、二进制表达分别表示

1. 首先**确定dp[i] 表示什么**
2. 找到dp[i] 和 dp[i - 1]...的关系
3. 找到dp[0]初始值

### 详细总结（摘自5.最长回文子串下weiwei 的题解，上传到网络记得标注一下）

![1f95da43d1bdeebdd1213bb804034ddc5f906dc61451cd63f2b5ab5d0eb33b33-「动态规划」问题思考方向](https://gitee.com/f0rest9999/images/raw/master/20201214143809.png)

1、思考状态（重点）

状态的定义，先尝试「题目问什么，就把什么设置为状态」；
然后思考「状态如何转移」，如果「状态转移方程」不容易得到，尝试修改定义，目的依然是为了方便得到「状态转移方程」。
「状态转移方程」是原始问题的不同规模的子问题的联系。即大问题的最优解如何由小问题的最优解得到。

2、思考状态转移方程（核心、难点）

状态转移方程是非常重要的，是动态规划的核心，也是难点；

常见的推导技巧是：分类讨论。即：对状态空间进行分类；

归纳「状态转移方程」是一个很灵活的事情，通常是具体问题具体分析；

除了掌握经典的动态规划问题以外，还需要多做题；

如果是针对面试，请自行把握难度。掌握常见问题的动态规划解法，理解动态规划解决问题，是从一个小规模问题出发，逐步得到大问题的解，并记录中间过程；

「动态规划」方法依然是「空间换时间」思想的体现，常见的解决问题的过程很像在「填表」。

3、思考初始化

初始化是非常重要的，一步错，步步错。初始化状态一定要设置对，才可能得到正确的结果。

角度 1：直接从状态的语义出发；

角度 2：如果状态的语义不好思考，就考虑「状态转移方程」的边界需要什么样初始化的条件；

角度 3：从「状态转移方程」方程的下标看是否需要多设置一行、一列表示「哨兵」（sentinel），这样可以避免一些特殊情况的讨论。

4、思考输出

有些时候是最后一个状态，有些时候可能会综合之前所有计算过的状态。

5、思考优化空间（也可以叫做表格复用）

「优化空间」会使得代码难于理解，且是的「状态」丢失原来的语义，初学的时候可以不一步到位。先把代码写正确是更重要；
「优化空间」在有一种情况下是很有必要的，那就是状态空间非常庞大的时候（处理海量数据），此时空间不够用，就必须「优化空间」；
非常经典的「优化空间」的典型问题是「0-1 背包」问题和「完全背包」问题。

####53 最大子序和

```markdown
给定一个整数数组 nums ，找到一个具有最大和的连续子数组（子数组最少包含一个元素），返回其最大和。

示例:

输入: [-2,1,-3,4,-1,2,1,-5,4]
输出: 6
解释: 连续子数组 [4,-1,2,1] 的和最大，为 6。
```

思路：用一个dp[]来记录目前为止最大的子序和  **选不选择将这个数加入之前的子序列中**

怎么判断选或者是不选这个数呢  

* 就看dp[i - 1]的值，如果< 0，则不选，因为总是满足 nums[i]  > nums[i] + dp[i - 1] 即这种情况是单独nums[i]单独算作一个序列

* 另外一种情况是，将nums[i] 加入其中，得到的dp[i] 一定大于 nums[i] 
* 这样就保证了dp[i]是目前i位置上最大的子序列和

```java
class Solution {
    public int maxSubArray(int[] nums) {
        if(nums.length == 0)
            return 0;
        int dp[] = new int [nums.length];
        dp[0] = nums[0];
        int max = dp[0];
        for(int i = 1;i < nums.length;i ++){
            if(dp[i - 1] < 0)
                dp[i] = nums[i];
            else
                dp[i] = nums[i] + dp[i - 1];
            max = Math.max(max, dp[i]);
        }
        return max;
    }
}
```

Test:

[-2,1,-3,4,-1,2,1,-5,4]

[-2 1 -2 4 3 5 6 1 5 ]

**优化解法**:即压缩了空间

```java
class Solution {
    public int maxSubArray(int[] nums) {
        if(nums.length == 0)
            return 0;
        int maxRes = nums[0];
        int maxSum = nums[0];
        for(int i = 1;i < nums.length;i++){
            if(maxSum < 0)
                maxSum = nums[i];
            else 
                maxSum += nums[i];
            maxRes = Math.max(maxRes, maxSum);
        }
        return maxRes;
    }
}
```



#### 70 爬楼梯

```markdown
假设你正在爬楼梯。需要 n 阶你才能到达楼顶。

每次你可以爬 1 或 2 个台阶。你有多少种不同的方法可以爬到楼顶呢？

注意：给定 n 是一个正整数。

示例 1：

输入： 2
输出： 2
解释： 有两种方法可以爬到楼顶。
1.  1 阶 + 1 阶
2.  2 阶
示例 2：

输入： 3
输出： 3
解释： 有三种方法可以爬到楼顶。
1.  1 阶 + 1 阶 + 1 阶
2.  1 阶 + 2 阶
3.  2 阶 + 1 阶
```

思路：dp[i]表示到目前为止的阶数有多少中走法。只能走两步或者是一步，即dp[i] 的状态和dp[i - 1]以及dp[i - 2]有关，列出状态转移方程：

​																				**dp[i] = dp[i - 1] + dp[i - 2]**

做初始化之后，计算即可

```java
class Solution {
    public int climbStairs(int n) {
        if(n == 0 || n == 1)
            return n;
        int dp[] = new int[n + 1];
        dp[1] = 1;
        dp[2] = 2;
        for(int i = 3;i < n + 1;i ++){
            dp[i] = dp[i - 1] + dp[i - 2];
        }
        return dp[n];
    }
}
```

**优化解法**：发现目前的状态只与之前相邻的两个状态有关系，即有思路来压缩空间

思路：可以维护一个大小为2的数组，有点滑动窗口的意思。

```java
class Solution {
    public int climbStairs(int n) {
        if(n == 0 || n == 1)
            return n;
        int temp []  = new int [2];
        temp[0] = 1;
        temp[1] = 2;
        for(int i = 3;i < n + 1;i ++){
            if(i % 2 == 0)
                temp[1] = temp[0] + temp[1];
            else
                temp[0] = temp[0] + temp[1];
        }
        return Math.max(temp[0], temp[1]);
    }
}
```



#### 121 买卖股票的最佳时机

```markdown
给定一个数组，它的第 i 个元素是一支给定股票第 i 天的价格。
如果你最多只允许完成一笔交易（即买入和卖出一支股票一次），设计一个算法来计算你所能获取的最大利润。
注意：你不能在买入股票前卖出股票。

示例 1:

输入: [7,1,5,3,6,4]
输出: 5
解释: 在第 2 天（股票价格 = 1）的时候买入，在第 5 天（股票价格 = 6）的时候卖出，最大利润 = 6-1 = 5 。
     注意利润不能是 7-1 = 6, 因为卖出价格需要大于买入价格；同时，你不能在买入前卖出股票。
示例 2:

输入: [7,6,4,3,1]
输出: 0
解释: 在这种情况下, 没有交易完成, 所以最大利润为 0。

```

思路：

dp[i]表示前i可以有的最大利润，应该是只为正数

除了第一天只可以买以外，其他的均可以选择今天是买入还是卖出，也正因为这样，dp[0] = 0，是默认值，所以我们直接跳过了

我们用一个minPrices来维护一个值，这个值代表目前为止最小的买入价格

可得状态方程是 dp[i] = Math.max(不卖出， 卖出)

不卖出是从上一个状态转移过来的，卖出的话就是目前的卖出价减去最小的买入价格

```java
class Solution {
    public int maxProfit(int[] prices) {
        if(prices.length == 0)
            return 0;
        int dp[] = new int [prices.length];
        int minPrices = prices[0];
        for(int i = 1;i < prices.length;i++){
            minPrices = Math.min(minPrices, prices[i]);
            dp[i] = Math.max(dp[i - 1], prices[i] - minPrices);
        }
        return dp[prices.length - 1];
    }
}
```

Test:

[7,1,5,3,6,4]

[0,0,4,4,5,5]

思路：

我们发现dp[i] 只和 dp[i - 1]有关系，**因此可以考虑将O(n)的空间复杂度降到O(1)**，实际上这也是能做到的

我们用maxProfit来记录之前最大的利润值

```java
class Solution {
    public int maxProfit(int[] prices) {
        if(prices.length == 0)
            return 0;
        int maxProfit = 0;
        int minPrices = Integer.MAX_VALUE;
        for(int i = 0;i < prices.length;i++){
            maxProfit = Math.max(maxProfit, prices[i] - minPrices);
        	minPrices = Math.min(minPrices, prices[i]);
        }
        return maxProfit;
    }
}
```

Test:

[7,1,5,3,6,4]

0 0 4 4 5 5 

**可以看出只是压缩了空间，思路是完全一样的。**

通过这个题会发现其实有些DP的题完全可以尝试进行压缩空间，所以重新优化了一下53题和73题。以后做的时候也要有这个意识。



#### 198 打家劫舍

```markdown
你是一个专业的小偷，计划偷窃沿街的房屋。每间房内都藏有一定的现金，影响你偷窃的唯一制约因素就是相邻的房屋装有相互连通的防盗系统，如果两间相邻的房屋在同一晚上被小偷闯入，系统会自动报警。

给定一个代表每个房屋存放金额的非负整数数组，计算你 不触动警报装置的情况下 ，一夜之内能够偷窃到的最高金
示例 1：

输入：[1,2,3,1]
输出：4
解释：偷窃 1 号房屋 (金额 = 1) ，然后偷窃 3 号房屋 (金额 = 3)。
     偷窃到的最高金额 = 1 + 3 = 4 。
示例 2：

输入：[2,7,9,3,1]
输出：12
解释：偷窃 1 号房屋 (金额 = 2), 偷窃 3 号房屋 (金额 = 9)，接着偷窃 5 号房屋 (金额 = 1)。
     偷窃到的最高金额 = 2 + 9 + 1 = 12 。
```

思路：走到每个房屋，有两种选择，选择还是不选。它的状态直接来自于之前的房屋选没选择。

**用dp(i, 0)表示i屋不选择的最大金额，dp(i, 1)表示i屋选择的最大金额**

dp(i, 0)由之前的dp(i - 1, 0) 或者 是dp(i - 1, 1)转移过来

dp(i, 1)只能由之前的dp(i - 1, 0) 转移过来 因为不能连续的选择两个房屋，截至最大值就是dp(i - 1, 0) + 当前值

```java
class Solution {
    public int rob(int[] nums) {
        int n = nums.length;
        if(n == 0)
            return 0;
        if(n == 0)
            return nums[0];
        int dp[][] = new int [n][2];
        dp[0][0] = 0;
        dp[0][1] = nums[0];
        for(int i = 1;i < n;i++){
            dp[i][0] = Math.max(dp[i - 1][0], dp[i - 1][1]);
            dp[i][1] = dp[i - 1][0] + nums[i];
        }
        return Math.max(dp[n - 1][0], dp[n - 1][1]);
    }
}
```

Test:

[1,2,3,1]

[0 1 

1 2 

2 4 

4 3]

![image-20201111090905972](C:\Users\CHENG\AppData\Roaming\Typora\typora-user-images\image-20201111090905972.png)

**优化解法：其实发现也是可以进行压缩的**

思路：整理思路其实发现不用二维数组的，因为dp(i, 0) 和 dp(i, 1) 只用保持一个max值就好了

dp[i] 表示到目前为止能拿到的最大金额

dp[i] 只能由两个状态转移过来，第一个是dp[i - 2] + 当前值 ，第二个是dp[i - 1]，分别对应选择与不选择的情况

我们保持dp[i] 取这两个值中较大的那个

```java
class Solution {
    public int rob(int[] nums) {
        int n = nums.length;
        if(n == 0)
            return 0;
        if(n == 1)
            return nums[0];
        int dp[] = new int [n];
        dp[0] = nums[0];
        dp[1] = Math.max(nums[0], nums[1]);
        for(int i = 2;i < n;i++){
            dp[i] = Math.max(dp[i - 2] + nums[i], dp[i - 1]);
        }
        return dp[n - 1];
    }
}
```

Test:

[1,2,3,1]

[1 2 4 4] 可以看到保存了第一个题解的每行的最大值

**再优化解法：发现还是可以进行压缩，即当前状态只和前两次状态有关系，我们只需要保存前两次状态即可**

dp[0] 存偶数下标的值，dp[1]存奇数下标的值

状态转移方式与上述相同

```java
class Solution {
    public int rob(int[] nums) {
        int n = nums.length;
        if(n == 0)
            return 0;
        if(n == 1)
            return nums[0];
        int dp[] = new int [2];
        dp[0] = nums[0];
        dp[1] = Math.max(nums[0], nums[1]);
        for(int i = 2;i < n;i++){
            if(i % 2 == 0)
                dp[0] = Math.max(dp[0] + nums[i], dp[1]);
            else
                dp[1] = Math.max(dp[1] + nums[i], dp[0]);
        }
        return Math.max(dp[0], dp[1]);
    }
}
```

*感觉测试用例可能规模并不大，提升的效果有点玄学，leetcode的结果评测本身就有点玄学,但是肯定是越优化越好*！！



####213 打家劫舍2

```markdown
你是一个专业的小偷，计划偷窃沿街的房屋，每间房内都藏有一定的现金。这个地方所有的房屋都 围成一圈 ，这意味着第一个房屋和最后一个房屋是紧挨着的。同时，相邻的房屋装有相互连通的防盗系统，如果两间相邻的房屋在同一晚上被小偷闯入，系统会自动报警 。

给定一个代表每个房屋存放金额的非负整数数组，计算你 在不触动警报装置的情况下 ，能够偷窃到的最高金额。

示例 1：

输入：nums = [2,3,2]
输出：3
解释：你不能先偷窃 1 号房屋（金额 = 2），然后偷窃 3 号房屋（金额 = 2）, 因为他们是相邻的。
示例 2：

输入：nums = [1,2,3,1]
输出：4
解释：你可以先偷窃 1 号房屋（金额 = 1），然后偷窃 3 号房屋（金额 = 3）。
     偷窃到的最高金额 = 1 + 3 = 4 。
示例 3：

输入：nums = [0]
输出：0

提示：

1 <= nums.length <= 100
0 <= nums[i] <= 1000
```

思路：dp[i] 表示到目前为止能拿到的最大金额，这题与打家劫舍1不同的地方就只在于结尾处与开始处两个只能取一个。那我们就直接两种方式都算一遍，最后求最大值就好。

注意添加了n==2的特殊情况的判断

我们先计算第一个房屋正常偷的情况，然后计算到dp[n - 2]为止，因为dp[n - 1]不能偷，对结果也是没什么影响的

然后计算第一个房屋不偷，从第二个房屋开始正常偷的情况，计算到dp[n - 1]为止，然后比较两次计算结果的大小，取大的那个

实际的计算过程和1的是完全一样的，就是加了一些边界条件。

```java
class Solution {
    public int rob(int[] nums) {
        int n = nums.length;
        if(n == 0)
            return 0;
        if(n == 1)
            return nums[0];
        if(n == 2)
            return Math.max(nums[0], nums[1]);
        int dp[] = new int[n];
        dp[0] = nums[0];
        dp[1] = Math.max(nums[0], nums[1]);
        for(int i = 2;i < n - 1;i++){
            dp[i] = Math.max(dp[i - 2] + nums[i], dp[i - 1]);
        }
        int res1 = dp[n - 2];
        dp[0] = 0;
        dp[1] = nums[1];
        for(int i = 2;i < n;i++){
            dp[i] = Math.max(dp[i - 2] + nums[i], dp[i - 1]);
        }
        int res2 = dp[n - 1];
        return Math.max(res1, res2);
    }
}
```



#### 337 打家劫舍3

```markdo
在上次打劫完一条街道之后和一圈房屋后，小偷又发现了一个新的可行窃的地区。这个地区只有一个入口，我们称之为“根”。 除了“根”之外，每栋房子有且只有一个“父“房子与之相连。一番侦察之后，聪明的小偷意识到“这个地方的所有房屋的排列类似于一棵二叉树”。 如果两个直接相连的房子在同一天晚上被打劫，房屋将自动报警。

计算在不触动警报的情况下，小偷一晚能够盗取的最高金额。

示例 1:
输入: [3,2,3,null,3,null,1]

     3
    / \
   2   3
    \   \ 
     3   1

输出: 7 
解释: 小偷一晚能够盗取的最高金额 = 3 + 3 + 1 = 7.
示例 2:

输入: [3,4,5,1,3,null,1]

     3
    / \
   4   5
  / \   \ 
 1   3   1

输出: 9
解释: 小偷一晚能够盗取的最高金额 = 4 + 5 = 9.
```

![image-20201111164855943](C:\Users\CHENG\AppData\Roaming\Typora\typora-user-images\image-20201111164855943.png)

==**这个题是一道很好的动态规划和二叉树结合的题**==

**看完题解，发现思路并不是太难想，是我太懒了，没有好好思考，说不定可以想出来。**

```java
class Solution {
    //选中这个节点的map
    HashMap<TreeNode, Integer> pickIt = new HashMap<>();
    //没选中这个节点的的map
    HashMap<TreeNode, Integer> notPickIt = new HashMap<>();    
    public int rob(TreeNode root) {
        if(root == null)
            return 0;
        dfs(root);
        return Math.max(pickIt.get(root), notPickIt.get(root));
    }
    public void dfs(TreeNode root){
        if(root == null)
            return;
        if(root.left != null)
            dfs(root.left);
        if(root.right != null)
            dfs(root.right);
        if(root.left == null && root.right == null){
            pickIt.put(root, root.val);
            notPickIt.put(root, 0);
        }else{
            pickIt.put(root, root.val + notPickIt.getOrDefault(root.left, 0) + notPickIt.getOrDefault(root.right, 				0));
            notPickIt.put(root, Math.max(notPickIt.getOrDefault(root.left, 0), pickIt.getOrDefault(root.left, 0))
             + Math.max(notPickIt.getOrDefault(root.right, 0), pickIt.getOrDefault(root.right, 0)));
        }-
            
    }
}
```

**注：此题也可以优化空间，因为一个值只和之前的最多四个值有关系，但是栈空间的使用代价依旧是O(n)**



#### 152 乘积最大子数组

```markdown
给你一个整数数组 nums ，请你找出数组中乘积最大的连续子数组（该子数组中至少包含一个数字），并返回该子数组所对应的乘积。

示例 1:

输入: [2,3,-2,4]
输出: 6
解释: 子数组 [2,3] 有最大乘积 6。
示例 2:

输入: [-2,0,-1]
输出: 0
解释: 结果不能为 2, 因为 [-2,-1] 不是子数组。
```

思路：这个题主要难在负数和零的处理

**题解解法1：**

![image-20201111213748189](C:\Users\CHENG\AppData\Roaming\Typora\typora-user-images\image-20201111213748189.png)

```java
class Solution {
    public int maxProduct(int[] nums) {
        int n = nums.length;
        if(n == 0)
            return 0;
        int dpMax[] = new int[n];
        int dpMin[] = new int[n];
        dpMax[0] = nums[0];
        dpMin[0] = nums[0];
        int max = nums[0];
        for(int i = 1;i < n;i++){
            dpMax[i] = Math.max(dpMin[i - 1] * nums[i], Math.max(dpMax[i - 1] * nums[i], nums[i]));
            dpMin[i] = Math.min(dpMin[i - 1] * nums[i], Math.min(dpMax[i - 1] * nums[i], nums[i]));
            max = Math.max(max, dpMax[i]);
        }
        return max;
    }
}
```

**注：经典的老问题，可以压缩空间到常量，不压缩了，直接看第二种思路**

**题解解法2：这个是比较针对题目的解法，可以扩展思路用,比较巧妙**

其实对于这个问题的深挖，在有偶数个负数时，最大值就是累乘结果。

在奇数个负数时，

![image-20201111215121979](C:\Users\CHENG\AppData\Roaming\Typora\typora-user-images\image-20201111215121979.png)

即到目前为止，答案要么是累乘或者上述两种情况，上述这两种情况可以通过正序乘一遍，然后倒序乘一遍得到

然后偶数个负数情况其实涵盖在了上述两种情况的求解过程中了

最后要注意0

![image-20201111215409021](C:\Users\CHENG\AppData\Roaming\Typora\typora-user-images\image-20201111215409021.png)

```java
class Solution {
    public int maxProduct(int[] nums) {
        int n = nums.length;
        if(n == 0)
            return 0;
        int res = nums[0];
        int max = 1;
        for(int i = 0;i < n;i ++){
            max *= nums[i];
            res = Math.max(max, res);
            if(max == 0)
                max = 1; 
        }
        max = 1;
        for(int i = n - 1;i >= 0;i --){
            max *= nums[i];
            res = Math.max(max, res);
            if(max == 0)
                max = 1; 
        }
        return res;
    }
}
```



#### 5. 最长回文子串

```markdown
给定一个字符串 s，找到 s 中最长的回文子串。你可以假设 s 的最大长度为 1000。

示例 1：

输入: "babad"
输出: "bab"
注意: "aba" 也是一个有效答案。
示例 2：

输入: "cbbd"
输出: "bb"
```

思路：摘自题解

![image-20201112163503082](C:\Users\CHENG\AppData\Roaming\Typora\typora-user-images\image-20201112163503082.png)

```markdown
第 1 步：定义状态
dp[i][j] 表示子串 s[i..j] 是否为回文子串，这里子串 s[i..j] 定义为左闭右闭区间，可以取到 s[i] 和 s[j]。
```

```markdown
第 2 步：思考状态转移方程
在这一步分类讨论（根据头尾字符是否相等），根据上面的分析得到：

dp[i][j] = (s[i] == s[j]) and dp[i + 1][j - 1]
说明：
「动态规划」事实上是在填一张二维表格，由于构成子串，因此 i 和 j 的关系是 i <= j ，因此，只需要填这张表格对角线以上的部分。

看到 dp[i + 1][j - 1] 就得考虑边界情况。

边界条件是：表达式 [i + 1, j - 1] 不构成区间，即长度严格小于 2，即 j - 1 - (i + 1) + 1 < 2 ，整理得 j - i < 3。

这个结论很显然：j - i < 3 等价于 j - i + 1 < 4，即当子串 s[i..j] 的长度等于 2 或者等于 3 的时候，其实只需要判断一下头尾两个字符是否相等就可以直接下结论了。

如果子串 s[i + 1..j - 1] 只有 1 个字符，即去掉两头，剩下中间部分只有1个字符，显然是回文；
如果子串 s[i + 1..j - 1] 为空串，那么子串 s[i, j] 一定是回文子串。
因此，在 s[i] == s[j] 成立和 j - i < 3 的前提下，直接可以下结论，dp[i][j] = true，否则才执行状态转移。
```

```markdown
第 3 步：考虑初始化
初始化的时候，单个字符一定是回文串，因此把对角线先初始化为 true，即 dp[i][i] = true 。

事实上，初始化的部分都可以省去。因为只有一个字符的时候一定是回文，dp[i][i] 根本不会被其它状态值所参考。
```

```markdown
第 4 步：考虑输出
只要一得到 dp[i][j] = true，就记录子串的长度和起始位置，没有必要截取，这是因为截取字符串也要消耗性能，记录此时的回文子串的「起始位置」和「回文长度」即可。
```

```markdown
第 5 步：考虑优化空间
因为在填表的过程中，只参考了左下方的数值。事实上可以优化，但是增加了代码编写和理解的难度，丢失可读和可解释性。在这里不优化空间。

注意事项：总是先得到小子串的回文判定，然后大子串才能参考小子串的判断结果，即填表顺序很重要。

大家能够可以自己动手，画一下表格，相信会对「动态规划」作为一种「表格法」有一个更好的理解。
```

**解法1 普通的动态规划：**

```java
class Solution {
    public String longestPalindrome(String s) {
        int length = s.length();
        if(length == 0 || length == 1)
            return s;
        int maxLength = 1;
        int start = 0;

        boolean dp[][] = new boolean [length][length];
        for(int i = 0;i < length;i ++){
            dp[i][i] = true;
        }
        //按列遍历，保证在dp[i][j]之前写过dp[i + 1][j - 1]
        for(int j = 1;j < length;j++){
            for(int i = 0;i < j;i++){
                if(s.charAt(i) != s.charAt(j))
                    dp[i][j] = false;
                else{
                    if(j - i < 3)
                        dp[i][j] = true;
                    else   
                        dp[i][j] = dp[i + 1][j - 1];
                }
                if(dp[i][j] && j - i + 1 > maxLength){
                    maxLength = j - i + 1;
                    start = i;
                }
            }
        }
        return s.substring(start, start + maxLength);
    }
}
```

**解法2 中心扩散法**

我们可以设计一个方法，兼容以上两种情况：

1、如果传入重合的索引编码，进行中心扩散，此时得到的回文子串的长度是奇数；

2、如果传入相邻的索引编码，进行中心扩散，此时得到的回文子串的长度是偶数。

具体编码细节在以下的代码的注释中体现。

```java
public class Solution {

    public String longestPalindrome(String s) {
        int len = s.length();
        if (len < 2) {
            return s;
        }
        int maxLen = 1;
        String res = s.substring(0, 1);
        // 中心位置枚举到 len - 2 即可
        for (int i = 0; i < len - 1; i++) {
            String oddStr = centerSpread(s, i, i);
            String evenStr = centerSpread(s, i, i + 1);
            String maxLenStr = oddStr.length() > evenStr.length() ? oddStr : evenStr;
            if (maxLenStr.length() > maxLen) {
                maxLen = maxLenStr.length();
                res = maxLenStr;
            }
        }
        return res;
    }

    private String centerSpread(String s, int left, int right) {
        // left = right 的时候，此时回文中心是一个字符，回文串的长度是奇数
        // right = left + 1 的时候，此时回文中心是一个空隙，回文串的长度是偶数
        int len = s.length();
        int i = left;
        int j = right;
        while (i >= 0 && j < len) {
            if (s.charAt(i) == s.charAt(j)) {
                i--;
                j++;
            } else {
                break;
            }
        }
        // 这里要小心，跳出 while 循环时，恰好满足 s.charAt(i) != s.charAt(j)，因此不能取 i，不能取 j
        return s.substring(i + 1, j);
    }
}
```



#### 62 不同路径

```markdown
一个机器人位于一个 m x n 网格的左上角 （起始点在下图中标记为“Start” ）。

机器人每次只能向下或者向右移动一步。机器人试图达到网格的右下角（在下图中标记为“Finish”）。

问总共有多少条不同的路径？

例如，上图是一个7 x 3 的网格。有多少可能的路径？


示例 1:

输入: m = 3, n = 2
输出: 3
解释:
从左上角开始，总共有 3 条路径可以到达右下角。
1. 向右 -> 向右 -> 向下
2. 向右 -> 向下 -> 向右
3. 向下 -> 向右 -> 向右
示例 2:

输入: m = 7, n = 3
输出: 28
```

dp(i,  j)表示到(i, j)为止一共有多少中走法

dp(i , j) = dp(i - 1, j) + dp(i , j - 1)

初始条件是dp(0, 0)是0， dp(0, j)都是1, dp(i, 0)都是 1

```java
class Solution {
    public int uniquePaths(int m, int n) {
        if(m == 1 && n == 1)
            return 1;
        int dp[][] = new int [m][n];
        for(int i = 0;i < m;i++){
            dp[i][0] = 1;
        }
        for(int j = 0;j < n;j++){
            dp[0][j] = 1;
        }
        for(int i = 1;i < m;i++){
            for(int j = 1;j < n;j++){
                dp[i][j] = dp[i - 1][j] + dp[i][j - 1];
            }
        }
        return dp[m - 1][n - 1];
    }
}
```



#### 63 不同路径2

```markdown
一个机器人位于一个 m x n 网格的左上角 （起始点在下图中标记为“Start” ）。

机器人每次只能向下或者向右移动一步。机器人试图达到网格的右下角（在下图中标记为“Finish”）。

现在考虑网格中有障碍物。那么从左上角到右下角将会有多少条不同的路径？

网格中的障碍物和空位置分别用 1 和 0 来表示。

示例 1：


输入：obstacleGrid = [[0,0,0],[0,1,0],[0,0,0]]
输出：2
解释：
3x3 网格的正中间有一个障碍物。
从左上角到右下角一共有 2 条不同的路径：
1. 向右 -> 向右 -> 向下 -> 向下
2. 向下 -> 向下 -> 向右 -> 向右
示例 2：


输入：obstacleGrid = [[0,1],[0,0]]
输出：1
```

 思路：与不同路径1不同的地方就在于从障碍物处出发没有可走方案，即障碍物的dp置0.即可

初始化时，也要注意有障碍物的情况

```java
class Solution {
    public int uniquePathsWithObstacles(int[][] obstacleGrid) {
        int m = obstacleGrid.length;
        int n = obstacleGrid[0].length;
        if(m == 1 && n == 1)
            return obstacleGrid[0][0] == 0 ? 1 : 0;
        int dp[][] = new int [m][n];
        for(int i = 0;i < m;i++){
            if(obstacleGrid[i][0] == 1) break;
            dp[i][0] = 1;
        }
        for(int j = 0;j < n;j++){
            if(obstacleGrid[0][j] == 1) break;
            dp[0][j] = 1;
        }
        for(int i = 1;i < m;i++){
            for(int j = 1;j < n;j++){
                if(obstacleGrid[i][j] == 1) continue;
                
                dp[i][j] = dp[i - 1][j] + dp[i][j - 1];
            }
        }
        return dp[m - 1][n - 1];
    }
}
```



#### 64 最小路径和

```markdown
给定一个包含非负整数的 m x n 网格 grid ，请找出一条从左上角到右下角的路径，使得路径上的数字总和为最小。

说明：每次只能向下或者向右移动一步。

示例 1：

输入：grid = [[1,3,1],[1,5,1],[4,2,1]]
输出：7
解释：因为路径 1→3→1→1→1 的总和最小。
示例 2：

输入：grid = [[1,2,3],[4,5,6]]
输出：12

提示：

m == grid.length
n == grid[i].length
1 <= m, n <= 200
0 <= grid[i][j] <= 100
```

思路：dp(i, j)表示到目前为止最小的路径总和

思路与不同路径大致相似

```java
class Solution {
    public int minPathSum(int[][] grid) {
        int n = grid.length;
        int m = grid[0].length;
        if(m == 1 && n == 1)
            return grid[0][0];
        int dp[][] = new int [n][m];
        dp[0][0] = grid[0][0];
        for(int i = 1;i < n;i++){
            dp[i][0] += dp[i - 1][0] + grid[i][0];
        }
        for(int j = 1;j < m;j++){
            dp[0][j] += dp[0][j - 1] + grid[0][j];
        }
        for(int i = 1;i < n;i++){
            for(int j = 1;j < m;j++){
                dp[i][j] = Math.min(dp[i - 1][j], dp[i][j - 1]) + grid[i][j];
            }
        }
        return dp[n - 1][m - 1];
    }
}
```

#### 221 最大正方形

```markdown
在一个由 '0' 和 '1' 组成的二维矩阵内，找到只包含 '1' 的最大正方形，并返回其面积。

示例：

输入：
matrix = [["1","0","1","0","0"],
          ["1","0","1","1","1"],
          ["1","1","1","1","1"],
          ["1","0","0","1","0"]]

输出：4
```

思路：用dp（i，j）表示以（i， j）为右下角的正方形的边长的最大值

如果该位置的值是0，dp（i，j）= 0,因为当前位置不可能在正方形中

**如果该位置是1，则由其dp的左上方、左方、上方的最小值决定**（参见1277题统计全为1的正方形子矩阵）

```java
class Solution {
    public int maximalSquare(char[][] matrix) {
        int n = matrix.length;
        if(n == 0)
            return 0;
        int m = matrix[0].length;
        int dp [][] = new int[n][m];
        int res = matrix[0][0] - 48;
        for(int i = 0;i < n;i++){
            dp[i][0] = matrix[i][0] - 48;
            res = Math.max(res, dp[i][0]);
        }
        for(int j = 0;j < m;j++){
            dp[0][j] = matrix[0][j] - 48;
            res = Math.max(res, dp[0][j]);
        }
        for(int i = 1;i < n;i++){
            for(int j = 1;j < m;j++){
                if(matrix[i][j] == 49){
                    dp[i][j] = Math.min(Math.min(dp[i - 1][j - 1], dp[i][j - 1]), dp[i - 1][j]) + 1;
                }
                res = Math.max(res, dp[i][j]);
            }
        }
        return res * res;
    }
}
```

#### 1277 统计全为1的正方形的子矩阵

```markdown
给你一个 m * n 的矩阵，矩阵中的元素不是 0 就是 1，请你统计并返回其中完全由 1 组成的 正方形 子矩阵的个数。

示例 1：
输入：matrix =
[
  [0,1,1,1],
  [1,1,1,1],
  [0,1,1,1]
]
输出：15
解释： 
边长为 1 的正方形有 10 个。
边长为 2 的正方形有 4 个。
边长为 3 的正方形有 1 个。
正方形的总数 = 10 + 4 + 1 = 15.
示例 2：

输入：matrix = 
[
  [1,0,1],
  [1,1,0],
  [1,1,0]
]
输出：7
解释：
边长为 1 的正方形有 6 个。 
边长为 2 的正方形有 1 个。
正方形的总数 = 6 + 1 = 7.
提示：

1 <= arr.length <= 300
1 <= arr[0].length <= 300
0 <= arr[i][j] <= 1
```

思路：思路和之前的题基本一样，此处给为何状态转移方程可以这么写

![image-20201114140102606](https://gitee.com/f0rest9999/images/raw/master/20201214144549.png)

![image-20201114140153244](https://gitee.com/f0rest9999/images/raw/master/20201214144604.png)

```java
class Solution {
    public int countSquares(int[][] matrix) {
        int n = matrix.length;
        if(n == 0)
            return 0;
        int m = matrix[0].length;
        int dp [][] = new int[n][m];
        int sum = 0;
        for(int i = 0;i < n;i++){
            for(int j = 0;j < m;j++){
                if(i == 0 || j == 0)
                    dp[i][j] = matrix[i][j];
                else if(matrix[i][j] == 1){
                    dp[i][j] = Math.min(Math.min(dp[i - 1][j - 1], dp[i][j - 1]), dp[i - 1][j]) + 1;
                }
                else{
                    dp[i][j] = 0;
                }
                sum += dp[i][j];
            }
        }
        return sum;
    }
}
```

#### 264 丑数2

```markdown
编写一个程序，找出第 n 个丑数。

丑数就是质因数只包含 2, 3, 5 的正整数。

示例:

输入: n = 10
输出: 12
解释: 1, 2, 3, 4, 5, 6, 8, 9, 10, 12 是前 10 个丑数。
说明:  

1 是丑数。
n 不超过1690。
```

思路：初始化数组nums和三个指针（i2，i3，i5），循环计算所有丑数：算法很简单：在 2 * nums[i2]、3 * nums[i3]、5 * nums[i5]

选出最小的丑数并添加到数组中。并将该丑数对应的因子指针往前走一步。重复该步骤直到计算完 1690 个丑数。

![image-20201115102751379](https://gitee.com/f0rest9999/images/raw/master/20201214144625.png)

**这个题动态规划思路明显在什么地方呢：即当前之前所算出的得到的丑数就是之后要计算的丑数的一个因子，这样能全部保证，所有的数只和2，3，5有关系，每次得到的最小的那个数当作数组中接下来要添加进去的因子来进行添加**

```java
class Solution {
    public int nthUglyNumber(int n) {
        int nums [] = new int [1690];
        nums[0] = 1;
        int i2 = 0, i3 = 0, i5 = 0;
        for(int i = 1;i < n;i++){
            int min = Math.min(Math.min(nums[i2] * 2, nums[i3] * 3), nums[i5] * 5);
            if(min == nums[i2] * 2) i2++;
            if(min == nums[i3] * 3) i3++;
            if(min == nums[i5] * 5) i5++;
            nums[i] = min;
        }
        return nums[n - 1];
    }
}
```

#### 300 最长上升子序列

```markdown
给定一个无序的整数数组，找到其中最长上升子序列的长度。

示例:

输入: [10,9,2,5,3,7,101,18]
输出: 4 
解释: 最长的上升子序列是 [2,3,7,101]，它的长度是 4。
说明:

可能会有多种最长上升子序列的组合，你只需要输出对应的长度即可。
你算法的时间复杂度应该为 O(n2) 。
进阶: 你能将算法的时间复杂度降低到 O(n log n) 吗?
```

**思路：dp[i]定义为以nums[i]结尾的上升子序列的长度，这个定义中，nums[i]必须被选中，且是这个子序列的最后一个元素**

考虑状态转移方程：

遍历到 nums[i] 时，需要把下标 i 之前的所有的数都看一遍；

只要 nums[i] 严格大于在它位置之前的某个数，那么 nums[i] 就可以接在这个数后面形成一个更长的上升子序列；

因此，dp[i] 就等于下标 i 之前严格小于 nums[i] 的状态值的最大者 +1。

```java
class Solution {
    public int lengthOfLIS(int[] nums) {
    	int length = nums.length;
        if(length <= 1)
            return length;
        int dp[] = new int [length];
        dp[0] = 1;
        int MaxRes = 1;
        for(int i = 1;i < length;i ++){
            int max = 0;
            for(int j = 0;j < i;j++){
                if(nums[j] < nums[i]){
                    max = Math.max(max, dp[j]);
                }
            }
            dp[i] = max + 1;
            MaxRes = Math.max(MaxRes, dp[i]);
        }
        return MaxRes;
    }
}
```

**将算法的时间复杂度降低到 O(n log n)的优化解法思路：**

**贪心算法 + 二分查找**：**考虑一个简单的贪心，如果我们要使上升子序列尽可能的长，则我们需要让序列上升得尽可能慢，因此我们希望每次在上升子序列最后加上的那个数尽可能的小。**

基于上面的贪心思路，我们维护一个数组d[i],表示长度为i的最长上升子序列的末尾元素的最小值，用len记录目前最长的上升子序列的长度，起

始len为1，d[1] = nums[0]

 ![image-20201116143808123](../Typora%E7%AC%94%E8%AE%B0%E6%88%AA%E5%9B%BE/image-20201116143808123.png)		

最后整个算法流程就是：

设已经求出的最长上升子序列的长度为len，从前往后遍历数组nums,在遍历到nums[i]时，

如果nums[i] > d[len],则直接加入到d数组末尾，并更新len = len + 1;

否则 在d数组中二分查找，找到第一个比nums[i]小的数d[k]，并且更新d[k + 1] = nums[i]

```java
class Solution {
    public int lengthOfLIS(int[] nums) {
        int length = nums.length;
        if(length <= 1)
            return length;
    	int len = 1;
        int d [] = new int [length + 1];
       	d[1] = nums[0];
        for(int i = 1;i < length;i++){
            if(nums[i] > d[len]){
                d[++len] = nums[i];
            }else{
                int left = 1, right = len, pos = 0;
                while(left <= right){
                    int mid = (left + right) / 2;
                    if(d[mid] < nums[i]){
                        pos = mid;
                        left = mid + 1;
                    }else{
                        right = mid - 1;
                    }
                }
                d[pos + 1] = nums[i];
            }
        }
        return len;
    }	
}
```

Test:

```markdown
[10,9,2,5,3,7,101,18]
9 
2 
2 5 
2 3 
2 3 7 
2 3 7 101 
2 3 7 18 
```

第二个思路记一下！

#### 343 整数拆分

```markdo
给定一个正整数 n，将其拆分为至少两个正整数的和，并使这些整数的乘积最大化。 返回你可以获得的最大乘积。

示例 1:

输入: 2
输出: 1
解释: 2 = 1 + 1, 1 × 1 = 1。
示例 2:

输入: 10
输出: 36
解释: 10 = 3 + 3 + 4, 3 × 3 × 4 = 36。
说明: 你可以假设 n 不小于 2 且不大于 58。
```

思路：自己想的思路是，是没有什么明显的使用动态规划的，即能拆成3就拆成三，最后剩下的只能是0，1，2三种情况，0的时候不做处理，然后1的情况时，其实此时最大的乘积组合是3……34 而不是 3……331，然后剩下是2的时候就无所谓了直接乘就好（**这是记住的一个结论，有数学证明**）

```java
class Solution {
    public int integerBreak(int n) {
        if(n <= 3)
            return n - 1;
        int res = 1;
        while(n > 3){
            n -= 3;
            res *= 3;
        }
        if(n == 1){
            res *= 4;
            res /= 3;
        }else if(n != 0){
            res *= n;        
        }
        return res;
    }
}
```

思路2：**动态规划：由于每个正整数对应的最大乘积取决于比它小的正整数对应的最大乘积，因此可以使用动态规划求解。**

dp[i] 表示将整数i至少拆分为两个正整数的和所能得到的最大乘积，

dp[0] = dp[1] = 0, dp[2] = 1;

有两种情况：j 和 i - j相乘 即直接就是两个数相乘， 不然就是 j 和 dp[i - j] 即相当于把i - j再拆分

```java
class Solution {
    public int integerBreak(int n) {
        if(n <= 3)
            return n - 1;
        int dp[] = new int [n + 1];
        dp[2] = 1;
        for(int i = 3;i <= n;i++){
            int max = 0;
            for(int j = 1;j < i;j++){
                max = Math.max(Math.max(j * (i - j), j * dp[i - j]), max);
            }
            dp[i] = max;
        }
        return dp[n];
    }
}
```

 #### 剑指 Offer 14- I. 剪绳子

```markdown
与整数拆分一致
```

#### 392 判断子序列

```markdown
给定字符串 s 和 t ，判断 s 是否为 t 的子序列。

你可以认为 s 和 t 中仅包含英文小写字母。字符串 t 可能会很长（长度 ~= 500,000），而 s 是个短字符串（长度 <=100）。

字符串的一个子序列是原始字符串删除一些（也可以不删除）字符而不改变剩余字符相对位置形成的新字符串。（例如，"ace"是"abcde"的一个子序列，而"aec"不是）。

示例 1:
s = "abc", t = "ahbgdc"

返回 true.

示例 2:
s = "axc", t = "ahbgdc"

返回 false.

后续挑战 :

如果有大量输入的 S，称作S1, S2, ... , Sk 其中 k >= 10亿，你需要依次检查它们是否为 T 的子序列。在这种情况下，你会怎样改变代码？
```

思路一：双指针进行遍历

```java
class Solution {
    public boolean isSubsequence(String s, String t) {
        int i = 0;
        for(int j = 0;i < s.length() && j < t.length();){
            if(s.charAt(i) == t.charAt(j))
                i++;
            j++;
        }
        if(i == s.length())
            return true;
        return false;
    }
}
```

**思路二：动态规划思路**：**注意此题目中的DP数组是辅助的数组，并不是直接求出结果的数组，解决的是后续挑战**，通过观察我们发现，双指针思路，一直在j++，然后进行判断，这消耗了大量的时间，所以我们可以用DP来进行预处理，然后通过这个DP数组的记录，通过O(1)的时间跳到下一个要处理的字符

具体思路是，我们用一个dp数组来记录（i， j）表示从t字符串i处开始之后第一次出现 j 字符的位置，由此可知j所对应的列数是26，即26个字母，在进行状态转移的过程中，如果t中i位置的字符就是j，那么dp（i， j）= i，否则dp（i，j） = dp(i + 1, j) 

这样看来，我们应该是从前往后进行枚举才对，因为要先获得dp(i + 1, j) 的值，才能转移得到dp（i， j）的值

初始化时，可以将dp全部置-1进行初始化

```java
class Solution {
    public boolean isSubsequence(String s, String t) {
    	int length = t.length();
        if(length == 0) return s.length() == 0;
        int dp [][] = new int[length + 1][26];
        for(int a[] : dp){
            Arrays.fill(a, -1);
        }
        dp[length - 1][t.charAt(length - 1) - 'a'] = length - 1;
        for(int i = length - 2;i >= 0;i--){
            for(int j = 0;j < 26;j++){
                if(t.charAt(i) == j + 'a')
                    dp[i][j] = i;
                 else
                    dp[i][j] = dp[i + 1][j];
            }
        }
        int pos = 0;
        for(int i = 0;i < s.length();i++){
        	if(dp[pos][s.charAt(i) - 'a'] == -1)
                return false;
            pos = dp[pos][s.charAt(i) - 'a'] + 1;
        }
        return true;
    }
}
```

Test:   "abc"    "ahbgdc"

dp：0 2 5 4 -1 -1 3 1 -1 -1 -1 -1 -1 -1 -1 -1 -1 -1 -1 -1 -1 -1 -1 -1 -1 -1 

​		-1 2 5 4 -1 -1 3 1 -1 -1 -1 -1 -1 -1 -1 -1 -1 -1 -1 -1 -1 -1 -1 -1 -1 -1 

​		-1 2 5 4 -1 -1 3 -1 -1 -1 -1 -1 -1 -1 -1 -1 -1 -1 -1 -1 -1 -1 -1 -1 -1 -1 

​		-1 -1 5 4 -1 -1 3 -1 -1 -1 -1 -1 -1 -1 -1 -1 -1 -1 -1 -1 -1 -1 -1 -1 -1 -1

​		-1 -1 5 4 -1 -1 -1 -1 -1 -1 -1 -1 -1 -1 -1 -1 -1 -1 -1 -1 -1 -1 -1 -1 -1 -1

​		-1 -1 5 -1 -1 -1 -1 -1 -1 -1 -1 -1 -1 -1 -1 -1 -1 -1 -1 -1 -1 -1 -1 -1 -1 -1

​		-1 -1 -1 -1 -1 -1 -1 -1 -1 -1 -1 -1 -1 -1 -1 -1 -1 -1 -1 -1 -1 -1 -1 -1 -1 -1

#### 368 最大整除子集

```markdown
给出一个由无重复的正整数组成的集合，找出其中最大的整除子集，子集中任意一对 (Si，Sj) 都要满足：Si % Sj = 0 或 Sj % Si = 0。

如果有多个目标子集，返回其中任何一个均可。
示例 1:

输入: [1,2,3]
输出: [1,2] (当然, [1,3] 也正确)
示例 2:

输入: [1,2,4,8]
输出: [1,2,4,8]
```

思路：在开始写DP算法之前，我们先看几个推论

给定升序序列（即 `E < F < G`）`[E, F, G]`，并且该列表本身满足问题中描述的整除子集，就不必枚举该子集的所有数字：

推论一：可除以整除子集中的最大元素的任何值，加入到子集中，可以形成另一个整除子集，即对于所有 h，若有 h % G == 0，则 [E, F, G, h] 形成新的可除子集。

即首先进行排序，利用推论一，然后开始遍历，对于每个数找到目前子集的最后一个数可以被整数的情况，并且加入到目前的子集中，保留一个最大长度，最后进行遍历一遍然后输出一个最长度的子集。

下面给出带注释的代码

```java
class Solution {
    public List<Integer> largestDivisibleSubset(int[] nums) {
        //dp[i] 代表以nums[i]结尾的，最长的子集
        //是一个List类型
        //告诉我们其实dp数组的类型不一定非要是常用类型，也可能是容器类型，只要使用得当，同样可以实现DP
        Arrays.sort(nums);
        int n = nums.length;
        if(n == 0)  return new ArrayList<>();
        List []dp = new List[n];
        //初始化
        dp[0] = new ArrayList();
        dp[0].add(nums[0]);
        List<Integer> resList = dp[0];
        for(int i = 1;i < n;i++){
            dp[i] = new ArrayList();
            dp[i].add(nums[i]);
            for(int k = 0;k < i;k++){
                //如果这个数能够整除之前的某一个子集的最大整数，且之前的那个子集长
                //那么就用那个子集替换掉现在的dp[i] 并且将nums[i]加入 形成新的集合
                if(nums[i] % nums[k] == 0){
                    if(dp[i].size() < dp[k].size() + 1){
                        dp[i] = new ArrayList(dp[k]);
                        dp[i].add(nums[i]);
                    }
                }
            }
            //最后判断是否更新result
            if(resList.size() < dp[i].size())
                resList = dp[i];
        }
        return resList;
    }
}
```

#### 416 分割等和子集

```markdown
给定一个只包含正整数的非空数组。是否可以将这个数组分割成两个子集，使得两个子集的元素和相等。

注意:

每个数组中的元素不会超过 100
数组的大小不会超过 200
示例 1:

输入: [1, 5, 11, 5]

输出: true

解释: 数组可以分割成 [1, 5, 5] 和 [11].
 

示例 2:

输入: [1, 2, 3, 5]

输出: false

解释: 数组不能分割成两个元素和相等的子集。
```

思路：**入门级0-1背包问题**

**这个问题等价于什么，等价于数组中能不能挑选出一部分数，使得这些数的和等于整个数组元素和的一半！！！！**

然后明确dp（i， j）代表什么，代表从0 - i的这些数中是否可以挑出一些数满足它们的和为j的条件

因此确定dp是Boolean类型的，且是一个行大小为length的，列大小是target + 1的数组

特值判断：如果sum是奇数 或者目前数组中的最大的那个数比sum的一半还要大，直接返回false

初始化：在nums[0] <= target时，有dp（0,  nums[0]）= true，即第一个物品选择

状态转移：nums[i] == j 这一处直接判为选择， 并且continue

否则，dp（i， j）先复制过来dp（i - 1, j），即没有选择这个物品的情况，

其次再判断j > nums[i]时，看看dp（i - 1，j - nums[i]）处的值是否是true，即选择了这个物品的情况。

```java
class Solution {
    public boolean canPartition(int[] nums) {
        int length = nums.length;
        if(length <= 1)
            return false;
      	int sum = 0;
        int max = Integer.MIN_VALUE;
        for(int i : nums){
            sum += i;
            max = Math.max(max, i);
        }
        if(sum % 2 != 0 || max > sum / 2)
            return false;
       	int target = sum / 2;
        boolean dp [][] = new boolean [length][target + 1];
       	//dp[0][0] = false;
        if(nums[0] <= target)
            dp[0][nums[0]] = true;
        for(int i = 1;i < length;i++){
            for(int j = 0;j <= target;j++){
                if(nums[i] == j){
                    dp[i][j] = true;
                    continue;
                }
                dp[i][j] = dp[i - 1][j];
                if(j > nums[i])
                	dp[i][j] = dp[i - 1][j] || dp[i - 1][j - nums[i]];
            }
        }
        return dp[length - 1][target];
    }
}
```

**空间优化解法（改天琢磨）**

#### 523 连续的子数组的和

```marks
给定一个包含 非负数 的数组和一个目标 整数 k，编写一个函数来判断该数组是否含有连续的子数组，其大小至少为 2，且总和为 k 的倍数，即总和为 n*k，其中 n 也是一个整数。

示例 1：

输入：[23,2,4,6,7], k = 6
输出：True
解释：[2,4] 是一个大小为 2 的子数组，并且和为 6。
示例 2：

输入：[23,2,6,4,7], k = 6
输出：True
解释：[23,2,6,4,7]是大小为 5 的子数组，并且和为 42。
说明：

数组的长度不会超过 10,000 。
你可以认为所有数字总和在 32 位有符号整数范围内。
```

思路：

特殊情况判断：如果有连续的两个0出现，则直接输出true，除此之外 k==0 时直接输出false

设置一个dp来存储到目前为止的数的和的值

然后双重循环 遍历 每次算出  从i到j的和 看这个和是否能整除k

具体算法就是拿 dp[j] - dp[i] + nums[i] 表示从i到j所有数的和，即连续子数组

```java
class Solution {
    public boolean checkSubarraySum(int[] nums, int k) {
        int length = nums.length;
        if(length <= 1)
            return false;
        int dp[] = new int [length];
        dp[0] = nums[0];
        for(int i = 1;i < length;i++){
            if(nums[i] == 0 && nums[i - 1] == 0)
                return true;
            dp[i] = dp[i - 1] + nums[i];
        }
        if(k == 0)  return false;
        for(int i = 0;i < length - 1;i++){
            for(int j = i + 1;j < length;j++){
            	int p = dp[j] - dp[i] + nums[i];    
            	if(p % k == 0)
                	return true;
            }
        }
        return false;
    }
}
```

#### 413 等差数列划分

```markdo
如果一个数列至少有三个元素，并且任意两个相邻元素之差相同，则称该数列为等差数列。

例如，以下数列为等差数列:

1, 3, 5, 7, 9
7, 7, 7, 7
3, -1, -5, -9
以下数列不是等差数列。
1, 1, 2, 5, 7

数组 A 包含 N 个数，且索引从0开始。数组 A 的一个子数组划分为数组 (P, Q)，P 与 Q 是整数且满足 0<=P<Q<N 。

如果满足以下条件，则称子数组(P, Q)为等差数组：

元素 A[P], A[p + 1], ..., A[Q - 1], A[Q] 是等差的。并且 P + 1 < Q 。

函数要返回数组 A 中所有为等差数组的子数组个数。
示例:

A = [1, 2, 3, 4]

返回: 3, A 中有三个子等差数组: [1, 2, 3], [2, 3, 4] 以及自身 [1, 2, 3, 4]。
```

思路：注意题目中的等差数列划分，必须是按照数组的原数据进行划分的，所以不用考虑跳着划分的情况，那样太难了

有了这个铺垫我们就可以直接开始了，如果 A[i] - A[i - 1] = A[i - 1] - A[i - 2] 那么这三个数构成等差数列

创建一个长度为length的dp数组，dp[i]表示 以i结尾的所有等差数列划分的个数

那么状态转移方程是什么：dp[i] = dp[i - 1 + 1，dp[i] 比dp[i - 1]多的那一个是什么

[1,2,3,4] [5]

原来应该是(2,3,4)(1,2,3,4)  然后在5处应该有(2,3,4,5)(1,2,3,4,5)    还有一个 (3,4,5)，即在之前的基础上每个加个5除外还有一个多出来的三元素的等差数列，所以总个数应该是dp[i - 1] + 1

```java
class Solution {
    public int numberOfArithmeticSlices(int[] A) {
		int length = A.length;
        if(length <= 2)	return 0;
        int dp [] = new int [length];
        int sum = 0;
        for(int i = 2;i < length;i++){
            if(A[i] - A[i - 1] == A[i - 1] - A[i - 2])
                dp[i] = dp[i - 1] + 1;
            sum += dp[i];
        }
        return sum;
    }
}
```

#### 673 最长递增序列的个数

```markdown
给定一个未排序的整数数组，找到最长递增子序列的个数。

示例 1:

输入: [1,3,5,4,7]
输出: 2
解释: 有两个最长递增子序列，分别是 [1, 3, 4, 7] 和[1, 3, 5, 7]。
示例 2:

输入: [2,2,2,2,2]
输出: 5
解释: 最长递增子序列的长度是1，并且存在5个子序列的长度为1，因此输出5。
注意: 给定的数组长度不超过 2000 并且结果一定是32位有符号整数。
```

思路1：假设对于nums[i]结尾的序列，设置一个dp[i]表示末尾元素是这个值的最长序列的长度，同时设置一个count[i]表示表示末尾元素是这个值的具有dp[i]长度的序列的个数

两重循环遍历这个数组，第一个遍历i   0 -> length - 1,第二个遍历j  0 -> i，保证了j < i

状态转移：我们只判断 nums[j] < nums[i]的这种情况，即满足最长子序列

如果dp[j] + 1 > dp[i] 即发现了新的最长子序列数：更新dp[i],count[i] = count[j] 

如果dp[j] + 1 == dp[i] 即没有发现新的最长子序列，那么现在的dp[i] 保持不变，然后count[i] += count[j]，即把满足这种条件的count加入到现在的count[i]中 

```java
class Solution {
    public int findNumberOfLIS(int[] nums) {
		int length = nums.length;
        if(length == 0) return 0;
        if(length == 1) return 1;
        int dp[] = new int[length];
        int count[] = new int[length];
      	Arrays.fill(dp, 1);
        Arrays.fill(count, 1);
        int max = 0;
        for(int i = 1;i < length;i++){
            for(int j = 0;j < i;j++){
                if(nums[i] > nums[j]){
                    if(dp[j] + 1 > dp[i]){
                        dp[i] = dp[j] + 1;
                        count[i] = count[j];
                    }else if(dp[j] + 1 == dp[i]){
                        count[i] += count[j];
                    }
                }
            }
            max = Math.max(max, dp[i]);
        }
        int res = 0;
        for(int i = 0;i < length;i++){
            if(dp[i] == max)
                res += count[i];
        }
        return res;
    }
}
```

思路2：线段树

#### 1143 最长公共子序列

```
给定两个字符串 text1 和 text2，返回这两个字符串的最长公共子序列的长度。

一个字符串的 子序列 是指这样一个新的字符串：它是由原字符串在不改变字符的相对顺序的情况下删除某些字符（也可以不删除任何字符）后组成的新字符串。
例如，"ace" 是 "abcde" 的子序列，但 "aec" 不是 "abcde" 的子序列。两个字符串的「公共子序列」是这两个字符串所共同拥有的子序列。

若这两个字符串没有公共子序列，则返回 0。
示例 1:

输入：text1 = "abcde", text2 = "ace" 
输出：3  
解释：最长公共子序列是 "ace"，它的长度为 3。
示例 2:

输入：text1 = "abc", text2 = "abc"
输出：3
解释：最长公共子序列是 "abc"，它的长度为 3。
示例 3:

输入：text1 = "abc", text2 = "def"
输出：0
解释：两个字符串没有公共子序列，返回 0。
 

提示:

1 <= text1.length <= 1000
1 <= text2.length <= 1000
输入的字符串只含有小写英文字符。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/longest-common-subsequence
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
```

思路：dp(i,j)表示text1的**长度为i**的前面的字符串和text2的**长度为j**的前面的字符串所拥有的最长的自序列的数值大小

当t1在 i 位置 的值等于t2 在j 位置的值时 ----->  dp(i, j) = dp(i - 1, j -1) + 1

当t1在 i 位置 的值不等于t2 在j 位置的值时 ----->  dp(i, j) = max(dp(i - 1, j),  dp(i, j - 1)) 

```java
class Solution {
    public int longestCommonSubsequence(String text1, String text2) {
		int length1 = text1.length();
        int length2 = text2.length();
        int dp[][] = new int [length1 + 1][length2 + 1];
        //对于这两个字符串，t2长度为0的时候值肯定是0
        for(int i = 0;i <= length1;i++){
            dp[i][0] = 0;
        }
        for(int i = 1;i <= length1;i++){
            char c = text1.charAt(i - 1);
            for(int j = 1;j <= length2;j++){
            	if(c == text2.charAt(j - 1))
                    dp[i][j] = dp[i - 1][j - 1] + 1;
                else
                    dp[i][j] = Math.max(dp[i - 1][j], dp[i][j - 1]);
            }
        }
        return dp[length1][length2];
    }
}
```

#### 120 三角形最小路径和

```
给定一个三角形，找出自顶向下的最小路径和。每一步只能移动到下一行中相邻的结点上。

相邻的结点 在这里指的是 下标 与 上一层结点下标 相同或者等于 上一层结点下标 + 1 的两个结点。

 

例如，给定三角形：

[
     [2],
    [3,4],
   [6,5,7],
  [4,1,8,3]
]
自顶向下的最小路径和为 11（即，2 + 3 + 5 + 1 = 11）。

 

说明：

如果你可以只使用 O(n) 的额外空间（n 为三角形的总行数）来解决这个问题，那么你的算法会很加分。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/triangle
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
```

思路：先写dp（i，j）表示移动到第 i 行第 j 列最小的路径和，最后比较最后一行的就可以了

```java
class Solution {
    public int minimumTotal(List<List<Integer>> triangle) {
		int row = triangle.size();
        if(row == 0) return 0;
        int col = triangle.get(row - 1).size();
        int[][] dp = new int[row + 1][col + 1];
        dp[1][1] = triangle.get(0).get(0);
        
        for(int i = 2;i <= row;i++){
            for(int j = 1;j <= triangle.get(i - 1).size();j ++){
                int min;
                if(j == 1)
                	min = dp[i - 1][1];
                else if(j == triangle.get(i - 1).size())
                    min = dp[i - 1][triangle.get(i - 2).size()];
                else
                    min = Math.min(dp[i - 1][j], dp[i - 1][j - 1]);
                dp[i][j] += triangle.get(i - 1).get(j - 1) + min;
            }
        }
        int res = Integer.MAX_VALUE;
        for(int i = 1;i <= col;i ++){
            res = Math.min(res, dp[row][i]);
        }
        return res;
    }
}
```

改进：可以发现只和上一行的数据有关系，其实设置一个数组就可以，这个数组不断地更新，最后剩下的那个数组就是最后一行的

```java
class Solution {
    public int minimumTotal(List<List<Integer>> triangle) {
		int row = triangle.size();
        if(row == 0) return 0;
        int col = triangle.get(row - 1).size();
        int[] dp = new int[col];
        dp[0] = triangle.get(0).get(0);
        
        for(int i = 1;i < row;i++){
            for(int j = triangle.get(i).size() - 1;j >= 0; j--){
                int min;
                if(j == 0)
                    min = dp[0];
                else if(j == triangle.get(i).size() - 1)
            		min = dp[j - 1];
             	else{
                    min = Math.min(dp[j], dp[j - 1]);
                }
                dp[j] = triangle.get(i).get(j) + min;
            }
        }
        int res = Integer.MAX_VALUE;
        for(int i = 0;i < col;i++){
            res = Math.min(res, dp[i]);
        }
        return res;
    }
}
```

![图画题解-5](https://gitee.com/f0rest9999/images/raw/master/20201212100358.jpg)

![image-20201212093650485](https://gitee.com/f0rest9999/images/raw/master/20201212093650.png)

#### 面试题01.05 一次编辑

```
字符串有三种编辑操作:插入一个字符、删除一个字符或者替换一个字符。 给定两个字符串，编写一个函数判定它们是否只需要一次(或者零次)编辑。

示例 1:

输入: 
first = "pale"
second = "ple"
输出: True
 
示例 2:

输入: 
first = "pales"
second = "pal"
输出: False

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/one-away-lcci
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
```

思路1：DP思路可以借鉴

dp(i，j)表示first的前i个长度的和second的前j个长度，目前为止做过几次编辑，删除操作和增加操作其实是一样的，对1删除就是对2增加可以这么理解，==但是DP在这个题表现很差==

```java
class Solution {
    public boolean oneEditAway(String first, String second) {
		int length1 = first.length();
        int length2 = second.length();
        if(Math.abs(length1 - length2) > 1)
            return false;
        int[][] dp = new int[length1 + 1][length2 + 1];
        for(int i = 1;i <= length1;i++){
            dp[i][0] = i;
        }
        for(int i = 1;i <= length2;i++){
            dp[0][i] = i;
        }
        for(int i = 1;i <= length1;i++){
            for(int j = 1;j <= length2;j++){
                if(first.charAt(i - 1) == second.charAt(j - 1))
                    dp[i][j] = dp[i - 1][j - 1];
                //考虑增删改
                else
                    dp[i][j] = Math.min(Math.min(dp[i - 1][j], dp[i][j - 1]), dp[i - 1][j - 1]) + 1;
            }
        }
        return dp[length1][length2] <= 1;
    }
}
```

思路2：在特殊情况判断之后，对比两个字符串通用下标的字符是否相同，第一次出现字符不同时采取相应的措施，第二次出现直接返回false

```java
class Solution {
    public boolean oneEditAway(String first, String second) {
		int length1 = first.length();
        int length2 = second.length();
        int difference = length1 - length2;
        if(difference > 1 || difference < -1)
            return false;
        char[] c1 = first.toCharArray();
        char[] c2 = second.toCharArray();
        boolean firstChance = false;
        for(int i = 0, j = 0;i < length1 && j < length2;i++, j++){
            if(c1[i] == c2[j])
                continue;
            else if(firstChance)
                return false;
            if(difference == 1)
                j --;
            else if(difference == -1)
                i --;
            firstChance = true;
        }
        return true;
    }
}
```

#### 91 解码方法

```
一条包含字母 A-Z 的消息通过以下方式进行了编码：

'A' -> 1
'B' -> 2
...
'Z' -> 26
给定一个只包含数字的非空字符串，请计算解码方法的总数。

题目数据保证答案肯定是一个 32 位的整数。

示例 1：

输入：s = "12"
输出：2
解释：它可以解码为 "AB"（1 2）或者 "L"（12）。
示例 2：

输入：s = "226"
输出：3
解释：它可以解码为 "BZ" (2 26), "VF" (22 6), 或者 "BBF" (2 2 6) 。
示例 3：

输入：s = "0"
输出：0
示例 4：

输入：s = "1"
输出：1
示例 5：

输入：s = "2"
输出：1

提示：

1 <= s.length <= 100
s 只包含数字，并且可能包含前导零。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/decode-ways
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
```

思路：==可以看做复杂化的爬楼梯==，转移方程比较好写，但是对于0这个数字的处理并不好处理，需要注意很多细节

我们知道正常情况下，dp[i] = dp[i - 1] + dp[i - 2];

算dp[i] 时，

1. 如果现在的数为0，那么加0，

   ​	否则加 dp[i - 1]

   这是计算的现在的数作为==单个数字==出现在最后面的情况，0 作为单个数字不代表任何一个字母

2. 再计算和之前的那个数字组合成==两位数==的情况，满足1 - 26的范围时就

   再加上dp[i - 2]

- 为了初始化方便，我们使用length + 1 长度的dp数组，道理是一样的

```java
class Solution {
    public int numDecodings(String s) {
        int length = s.length();
        if(length == 0) return 0;
        if(s.charAt(0) == '0')	return 0;
        
        int[] dp = new int[length + 1];
        dp[0] = 1;
        
        for(int i = 0;i < length;i++){
            dp[i + 1] = s.charAt(i) == '0' ? 0 : dp[i];
            if(i > 0 && (s.charAt(i - 1) == '1' || (s.charAt(i - 1) == '2' && s.charAt(i) <= '6')))
                dp[i + 1] += dp[i - 1];
        }
        
        return dp[length];
    }
}
```

#### 801 使序列递增的最小交换次数

```
我们有两个长度相等且不为空的整型数组 A 和 B 。

我们可以交换 A[i] 和 B[i] 的元素。注意这两个元素在各自的序列中应该处于相同的位置。

在交换过一些元素之后，数组 A 和 B 都应该是严格递增的（数组严格递增的条件仅为A[0] < A[1] < A[2] < ... < A[A.length - 1]）。

给定数组 A 和 B ，请返回使得两个数组均保持严格递增状态的最小交换次数。假设给定的输入总是有效的。

示例:
输入: A = [1,3,5,4], B = [1,2,3,7]
输出: 1
解释: 
交换 A[3] 和 B[3] 后，两个数组如下:
A = [1, 3, 5, 7] ， B = [1, 2, 3, 4]
两个数组均为严格递增的。
注意:

A, B 两个数组的长度总是相等的，且长度的范围为 [1, 1000]。
A[i], B[i] 均为 [0, 2000]区间内的整数。
```

思路：==注意题目中假定给定的输入总是有效的，也就是说如果在一个序列中不满足增，那么交换该位置之后必然满足两个序列都增==

我们用两个数组来表示，dp的第一列表示在i位置不换的时候，到目前为止最少交换次数，dp的第二列表示在i位置换的时候的最少交换次数

![图画题解-12](https://gitee.com/f0rest9999/images/raw/master/20201219154742.jpg)

```java
class Solution {
    public int minSwap(int[] A, int[] B) {
        int[][] dp = new int[A.length][2];
        //第一列表示不换
        dp[0][0] = 0;
        //第二列表示换
        dp[0][1] = 1;
        for(int i  = 1;i < A.length;i++){
            //有序
            if(A[i] > A[i - 1] && B[i] > B[i - 1]){
                //有交叉
                if(A[i] > B[i - 1] && B[i] > A[i - 1]){
                    //在i不换
                    dp[i][0] = Math.min(dp[i - 1][0], dp[i - 1][1]);
                    //在i换
                    dp[i][1] = Math.min(dp[i - 1][0], dp[i - 1][1]) + 1;
                }else{
                    dp[i][0] = dp[i - 1][0];
                    dp[i][1] = dp[i - 1][1] + 1;
                }
            }else{
                //无序必须换了 
                //要么换i 要么换i - 1
                dp[i][0] = dp[i - 1][1];
                dp[i][1] = dp[i - 1][0] + 1;
            }
        }
        return Math.min(dp[A.length - 1][0], dp[A.length - 1][1]);
    }
}
```

#### 131 分割回文串

```
给定一个字符串 s，将 s 分割成一些子串，使每个子串都是回文串。

返回 s 所有可能的分割方案。

示例:

输入: "aab"
输出:
[
  ["aa","b"],
  ["a","a","b"]
]

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/palindrome-partitioning
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
```

思路：这题是较好的动态规划和回溯结合的题，主要思路是回溯遍历所有可以剪切的位置，如果每一步都剪切成功，最后加入结果，如果有一步没有剪切，成功则完成剪枝，该分支不用再计算

先用动态规划计算好dp，dp（i， j）表示s中i到j的子字符串是否是回文的，这样我们在回溯过程中，只用O（1）的时间来找到其是否满足回文，参考[5. 最长回文子串](https://leetcode-cn.com/problems/longest-palindromic-substring/)

再每次回溯的过程中，我们定义一个start，表示开始截取的位置，然后循环遍历所有可以遍历的位置，前提是dp中该对应值为true

```java
class Solution {
    //动态规划优化 + 回溯
    //思路是正确的
    private List<List<String>> resList = new ArrayList<>();
    private boolean[][] dp;
    public List<List<String>> partition(String s) {
        int len = s.length();
        if(len == 0)    return resList;
        dp = new boolean [len][len];
        for(int i = 0;i < len;i++)
            dp[i][i] = true;
        for(int j = 1;j < len;j++){
            for(int i = 0;i < j;i++){
                if(s.charAt(i) != s.charAt(j))
                    continue;
                else{
                    if(j - i < 3)
                        dp[i][j] = true;
                    else
                        dp[i][j] = dp[i + 1][j - 1];
                }
            }
        }
        Deque<String> path = new ArrayDeque<>();
        dfs(s, path, 0, len);
        return resList;
    }
    public void dfs(String s, Deque<String> path, int start, int len){
        if(start == len){
            resList.add(new ArrayList<String>(path));
            return ;
        }
        for(int i = start;i < len;i++){
            if(!dp[start][i])
                continue;
            path.addLast(s.substring(start, i + 1));
            dfs(s, path, i + 1, len);
            path.removeLast();
        }
    }
}
```

#### 279 完全平方数

```
给定正整数 n，找到若干个完全平方数（比如 1, 4, 9, 16, ...）使得它们的和等于 n。你需要让组成和的完全平方数的个数最少。

示例 1:

输入: n = 12
输出: 3 
解释: 12 = 4 + 4 + 4.
示例 2:

输入: n = 13
输出: 2
解释: 13 = 4 + 9.
```

思路：这个题可以特殊处理

- 我们用一个list来按顺序存储所有小于n的平方数，最开始里面只有一个1
- 其次我们用dp[i]表示i最少可以用几个平方数构成,其中所有平方数的dp[i]初始化为1
- 从2开始，对于不是平方数的值，我们遍历list，来找到最小的dp[j] + dp[i - j]，代表着一个数是平方数，另一个数是i - j所代表的差值，这个在之前已经计算过了。（这里特殊利用了数组是升序的性质）
- 对于是平方数的值，我们加入list，并且continue

```java
class Solution {
    public int numSquares(int n) {
        if(n <= 1) return 1;
        List<Integer> list = new LinkedList<>();
        list.add(1);
        int[] dp = new int[n + 1];
        Arrays.fill(dp, Integer.MAX_VALUE);
        for(int i = 1;i * i <= n;i++)
            dp[i * i] = 1;
        for(int i = 2;i <= n;i++){
            if(dp[i] == 1){
                list.add(i);
                continue;
            }
            for(int j : list){
                dp[i] = Math.min(dp[i], dp[j] + dp[i - j]);
            }
        }
        return dp[n]; 
    }
}
```

Test：13

index:           0           1 2 3 4 5 6 7 8 9 10  11  12  13

结果：2147483647 1 2 3 1 2 3 4 2  1   2   3    3    2 





## 二叉树

### 总结

```java
Definition for a binary tree node.
public class TreeNode {
    int val;
    TreeNode left;
    TreeNode right;
    TreeNode() {}
    TreeNode(int val) { this.val = val; }
    TreeNode(int val, TreeNode left, TreeNode right) {
       this.val = val;
       this.left = left;
       this.right = right;
     }
}
```



####94 二叉树的中序遍历

```markdown
给定一个二叉树的根节点 root ，返回它的 中序 遍历。
```

思路：递归实现起来很简单，我们主要实现一下迭代的实现方法,中序遍历的迭代需要用辅助的栈

其实就是模拟了递归栈调用的过程

主要过程是 先向左一直走，然后，找到那个节点，加入，弹出，然后在其右子树上继续进行这个过程。

**注意第一个while的判断条件**

```java
class Solution {
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> resList = new ArrayList<Integer>();
        if(root == null)
        	return resList;
        Stack<TreeNode> stack = new Stack<>();
        while(root == null || !stack.isEmpty()){
            while(root != null){
                stack.push(root);
                root = root.left;
            }
            root = stack.pop();
            resList.add(root.val);
            root = root.right;
        }
        return resList;
    }
}
```

#### 95 不同的二叉搜索树2

```markdown
给定一个整数 n，生成所有由 1 ... n 为节点所组成的 二叉搜索树 。

示例：

输入：3
输出：
[
  [1,null,3,2],
  [3,2,null,1],
  [3,1,null,null,2],
  [2,1,3],
  [1,null,2,null,3]
]
解释：
以上的输出对应以下 5 种不同结构的二叉搜索树：

   1         3     3      2      1
    \       /     /      / \      \
     3     2     1      1   3      2
    /     /       \                 \
   2     1         2                 3
 
```

**思路：u1s1，最烦的就是做这种构造树的题，多总结一下怎么递归的，我总是想不到**

然后这个递归又是传递了相当于区间的参数。**递归的一个思维技巧就是假设问题已经解决了，你当前可以用递归的结果。**

大体上就是选择一个中间结点，左侧都是属于左子树的，右侧都是属于右子树的。分治一下，左侧的这些结点构成的各种树组成一个数组，右侧也是如此。最终从这两个数组中各取一个作为以中间结点为根节点的二叉搜索树。递归的一个思维技巧就是假设问题解决了，你当前可以用递归的结果。那么本问题中，递归的结果就是两侧的结点各自组成的不同二叉搜索树。另外一个就是收敛问题，不外乎是考虑start==end和start>end的情况，前者是还有一个结点，还能组成一棵树；后者是一棵空树。

```java
class Solution {
	public List<TreeNode> generateTrees(int n) {
        if(n == 0)
            return new LinkedList<TreeNode>();
        return generate(1, n);
    }
    public List<TreeNode> generate(int start, int end){
        List<TreeNode> resList = new ArrayList<>();
        if(start > end){
            resList.add(null);
            return resList;
        }
        for(int i = start;i <= end;i++){
            List<TreeNode> left = generate(start, i - 1);
            List<TreeNode> right = generate(i + 1, end);
            
            for(TreeNode l : left){
                for(TreeNode r : right){
                    TreeNode currentRoot = new TreeNode(i);
                    currentRoot.left = l;
                    currentRoot.right = r;
                    resList.add(currentRoot);
                }
            }
        }
        return resList;
    }
}
```

#### 98 验证二叉搜索树

```markdown
给定一个二叉树，判断其是否是一个有效的二叉搜索树。

假设一个二叉搜索树具有如下特征：

节点的左子树只包含小于当前节点的数。
节点的右子树只包含大于当前节点的数。
所有左子树和右子树自身必须也是二叉搜索树。
示例 1:

输入:
    2
   / \
  1   3
输出: true
示例 2:

输入:
    5
   / \
  1   4
     / \
    3   6
输出: false
解释: 输入为: [5,1,4,null,null,3,6]。
     根节点的值为 5 ，但是其右子节点值为 4 。
```

思路：**从根节点进行dfs,每到一个节点判断它和它的父节点之前满不满足二叉搜索树的性质，**如果满足则继续dfs它的子节点，如果不满足直接返回false。我们可以通过在递归函数中传区间参数进行判断当前节点和父节点是否满足二叉树性质。

```java
Class Solution{
    public boolean isValidBST(TreeNode root){
        return dfs(root, null, null);
    } 
    public boolean dfs(TreeNode root, Integer low_bound, Integer high_bound){
        if(root == null)
        	return true;
       	if(low_bound != null && low_bound >= root.val)
            return false;
       	if(high_bound != null && high_bound <= root.val)
            return false;
       	if(!dfs(root.left, low_bound, root.val))
            return false;
        if(!dfs(root.right, root.val, high_bound))
            return false;
        return true;
    }
}
```

#### 100 相同的树

```markdown
给定两个二叉树，编写一个函数来检验它们是否相同。
如果两个树在结构上相同，并且节点具有相同的值，则认为它们是相同的。
示例 1:

输入:       1         1
          / \       / \
         2   3     2   3

        [1,2,3],   [1,2,3]

输出: true
示例 2:

输入:      1          1
          /           \
         2             2

        [1,2],     [1,null,2]

输出: false
示例 3:

输入:       1         1
          / \       / \
         2   1     1   2

        [1,2,1],   [1,1,2]

输出: false
```

思路：一个简单的递归函数

```java
class Solution {
    public boolean isSameTree(TreeNode p, TreeNode q) {
        if(p == null && q == null)	return true;
        if(p != null && q != null)
            return p.val == q.val && isSameTree(p.left, q.left) && isSameTree(p.right, q.right);
        return false;
    }
}
```

#### 101 对称的树

```markdo
给定一个二叉树，检查它是否是镜像对称的。

例如，二叉树 [1,2,2,3,4,4,3] 是对称的。
    1
   / \
  2   2
 / \ / \
3  4 4  3

但是下面这个 [1,2,2,null,3,null,3] 则不是镜像对称的:

    1
   / \
  2   2
   \   \
   3    3
```

思路：一个简单的递归函数

```java
class Solution {
    public boolean isSymmetric(TreeNode root) {
        if(root == null)	return true;
        return judge(root.left, root.right);
    }
    public boolean judge(TreeNode left, TreeNode right){
        if(left == null && right == null)	return true;
        if(left != null && right != null){
            return left.val == right.val && judge(left.left, right.right) && judge(left.right, right.left);
        }
        return false;
    }
}
```

#### 102 层次遍历

```markdown
给你一个二叉树，请你返回其按 层序遍历 得到的节点值。 （即逐层地，从左到右访问所有节点）。

示例：
二叉树：[3,9,20,null,null,15,7],

    3
   / \
  9  20
    /  \
   15   7
返回其层次遍历结果：

[
  [3],
  [9,20],
  [15,7]
]
```

思路：用Queue做，注意一个细节，就是for(int i = queue.size();i > 0;i --)的开始条件为什么要反着过来，因为如果queue的size在不断的poll和add过程中会变化

```java
class Solution {
    public List<List<Integer>> levelOrder(TreeNode root) {
        if(root == null)
            return new LinkedList<>();
        Queue<TreeNode> queue = new LinkedList<>();
        List<List<Integer>> resList = new ArrayList<>();
        queue.add(root);
        while(!queue.isEmpty()){
            List<Integer> list = new ArrayList<>();
            for(int i = queue.size();i > 0;i --){
                TreeNode current = queue.poll();
                if(current != null){
                    queue.add(current.left);
                    queue.add(current.right);
                    list.add(current.val);
                }
            }
            if(!list.isEmpty())
                resList.add(list);
        }
        return resList;
    }
}

```

#### 104 二叉树的最大深度

```markdown
给定一个二叉树，找出其最大深度。

二叉树的深度为根节点到最远叶子节点的最长路径上的节点数。

说明: 叶子节点是指没有子节点的节点。

示例：
给定二叉树 [3,9,20,null,null,15,7]，

    3
   / \
  9  20
    /  \
   15   7
返回它的最大深度 3 。
```

思路：简单的递归

```java
class Solution {
    public int maxDepth(TreeNode root) {
		if(root == null)
            return 0;
        return Math.max(maxDepth(root.left), maxDepth(root.right)) + 1;
    }
}
```

#### 105 从前序与中序遍历序列构造二叉树

```markdown
根据一棵树的前序遍历与中序遍历构造二叉树。

注意:
你可以假设树中没有重复的元素。

例如，给出

前序遍历 preorder = [3,9,20,15,7]
中序遍历 inorder = [9,3,15,20,7]
返回如下的二叉树：

    3
   / \
  9  20
    /  \
   15   7
```

思路：前序遍历代表了根节点的位置，即先访问到的就是根节点，中序遍历代表了左右子树的值，我们这里用一个区间来限制它左子树应该放哪些，右子树应该放哪些，为了方便的找到当前根节点所对应的左右子树，我们要定位它在inorder中的值，避免每次都遍历寻找，我们可以空间换时间，用一个专门的map进行保存。

递归：每遍历一个数值，我们首先创建一个TreeNode，然后看以它为根节点看它的左子树以及右子树，再一次次递归中缩小的是区间长度，就相当于**后序遍历**一样，我们访问完一个位置是preOrder_index的节点，那么它左子树的根节点就是preOrder_index + 1，右子树的根节点就是preOrder_index + + indexOfInOrder - left + 1，然后区间就相当于在InOrder上缩小了。

最后构造出来一个树

```java
class Solution {
    public int [] preorder;
    public Map<Integer, Integer> posOfInOrder;
    public TreeNode buildTree(int[] preorder, int[] inorder) {
        this.preorder = preorder;
        posOfInOrder = new HashMap<>();
        for(int i = 0;i < inorder.length;i++){
            posOfInOrder.put(inorder[i], i);
        }
        return make(0, 0, preorder.length - 1);
    }
    public TreeNode make(int preOrder_index, int left, int right){
        if(left > right) return null;
        int root_val = preorder[preOrder_index];
        TreeNode root = new TreeNode(root_val);
        int indexOfInOrder = posOfInOrder.get(root_val);
        root.left = make(preOrder_index + 1, left, indexOfInOrder - 1);
        root.right = make(preOrder_index + indexOfInOrder - left + 1, indexOfInOrder + 1, right);
    	return root;
    }
}
```

#### 106 从后序与中序遍历序列构造二叉树

```markdown
根据一棵树的中序遍历与后序遍历构造二叉树。

注意:
你可以假设树中没有重复的元素。

例如，给出

中序遍历 inorder = [9,3,15,20,7]
后序遍历 postorder = [9,15,7,20,3]
返回如下的二叉树：

    3
   / \
  9  20
    /  \
   15   7
```

思路：与上题相似，区别仅在于，从postorder后面数是根节点，以及区间端点的位置，画个例子就可以清晰的看出来了

```java
class Solution {
    public int [] postorder;
    public Map<Integer, Integer> posOfInOrder;
    public TreeNode buildTree(int[] inorder, int[] postorder) {
		this.postorder = postorder;
        posOfInOrder = new HashMap<>();
        for(int i = 0;i < inorder.length;i++){
            posOfInOrder.put(inorder[i], i);
        }
        return make(postorder.length - 1, 0, postorder.length - 1);
    }
    public TreeNode make(int postorder_index, int left, int right){
        if(left > right) return null;
        int root_val = postorder[postorder_index];
        TreeNode root = new TreeNode(root_val);
        int indexOfInOrder = posOfInOrder.get(root_val);
        root.left = make(postorder_index + indexOfInOrder - 1 - right, left, indexOfInOrder - 1);
        root.right = make(postorder_index - 1, indexOfInOrder + 1, right);
    	return root;
    }
}
```

Test: [9,3,15,20,7]

​		  [9,15,7,20,3]

结果：[3,9,20,null,null,15,7]

#### 107 二叉树的层序遍历

```markdown
给定一个二叉树，返回其节点值自底向上的层次遍历。 （即按从叶子节点所在层到根节点所在的层，逐层从左向右遍历）

例如：
给定二叉树 [3,9,20,null,null,15,7],

    3
   / \
  9  20
    /  \
   15   7
返回其自底向上的层次遍历为：

[
  [15,7],
  [9,20],
  [3]
]
```

思路：就懒惰地加一个Collections.reverse(resList)

```java
class Solution {
    public List<List<Integer>> levelOrderBottom(TreeNode root) {
        if(root == null)
            return new LinkedList<>();
        Queue<TreeNode> queue = new LinkedList<>();
        List<List<Integer>> resList = new ArrayList<>();
        queue.add(root);
        while(!queue.isEmpty()){
            List<Integer> list = new ArrayList<>();
            for(int i = queue.size();i > 0;i --){
                TreeNode current = queue.poll();
                if(current != null){
                    queue.add(current.left);
                    queue.add(current.right);
                    list.add(current.val);
                }
            }
            if(!list.isEmpty())
                resList.add(list);
        }
        Collections.reverse(resList);
        return resList;
    }
}
```

#### 108 将有序数组转换为二叉搜索树

```markdown
将一个按照升序排列的有序数组，转换为一棵高度平衡二叉搜索树。

本题中，一个高度平衡二叉树是指一个二叉树每个节点 的左右两个子树的高度差的绝对值不超过 1。

示例:

给定有序数组: [-10,-3,0,5,9],

一个可能的答案是：[0,-3,9,-10,null,5]，它可以表示下面这个高度平衡二叉搜索树：

      0
     / \
   -3   9
   /   /
 -10  5
```

思路：这是构造树例子中最简单的一道，其实大体上思路都一样的

```java
class Solution {
    public int nums[];
    public TreeNode sortedArrayToBST(int[] nums) {
		this.nums = nums;
        return make(0, nums.length - 1);
    }
    public TreeNode make(int left, int right){
        if(left > right) return null;
        int mid = (left + right + 1) / 2;
        TreeNode root = new TreeNode(nums[mid]);
        root.left = make(left, mid - 1);
        root.right = make(mid + 1, right);
        return root;
    }
}
```

#### 110 平衡二叉树

```markdown
给定一个二叉树，判断它是否是高度平衡的二叉树。

本题中，一棵高度平衡二叉树定义为：

一个二叉树每个节点 的左右两个子树的高度差的绝对值不超过 1 。

示例 1：
输入：root = [3,9,20,null,null,15,7]
输出：true

示例 2：
输入：root = [1,2,2,3,3,null,null,4,4]
输出：false

示例 3：
输入：root = []
输出：true
```

思路：这个height所返回的结果可以这么理解

```java
class Solution {
    public boolean isBalanced(TreeNode root) {
        if(root == null) return true;
        return height(root) >= 0;
    }
    public int height(TreeNode root){
        if(root == null) return 0;
        int leftHeight = height(root.left);
        int rightHeight = height(root.right);
       	if(leftHeight == -1 || rightHeight == -1 || Math.abs(leftHeight - rightHeight) > 1)
            return -1;
        return Math.max(leftHeight, rightHeight) + 1;
    }
}
```

#### 113 路径总和 2

```markdown
给定一个二叉树和一个目标和，找到所有从根节点到叶子节点路径总和等于给定目标和的路径。
说明: 叶子节点是指没有子节点的节点。
示例:
给定如下二叉树，以及目标和 sum = 22，

              5
             / \
            4   8
           /   / \
          11  13  4
         /  \    / \
        7    2  5   1
返回:

[
   [5,4,11,2],
   [5,8,4,5]
]
```

思路：这题真给我写吐了，**没搞懂为什么用list就不行**

```java
class Solution {
    List<List<Integer>> resList = new ArrayList<>();
    Deque<Integer> que = new LinkedList<>();
    public List<List<Integer>> pathSum(TreeNode root, int sum) {
        if(root == null) return resList;
        dfs(root, sum);
        return resList;
    }
    public void dfs(TreeNode root, int sum){
        if(root == null) return ;
        que.offerLast(root.val);
        sum -= root.val;        
        if(root.left == null && root.right == null && sum == 0){
            resList.add(new LinkedList<Integer>(que));
        }
        dfs(root.left, sum);
        dfs(root.right, sum);
        que.pollLast();
    }
}
```



#### 222 完全二叉树的节点个数

```markdown
给出一个完全二叉树，求出该树的节点个数。

说明：

完全二叉树的定义如下：在完全二叉树中，除了最底层节点可能没填满外，其余每层节点数都达到最大值，并且最下面一层的节点都集中在该层最左边的若干位置。若最底层为第 h 层，则该层包含 1~ 2h 个节点。

示例:

输入: 
    1
   / \
  2   3
 / \  /
4  5 6

输出: 6
```

思路：用一个笨方法，自己想的，但是好像也不慢

若是满二叉树的话节点个数是 2^n - 1，先遍历找到层数，然后遍历所有节点，定位倒数第二层的节点，然后判断，因为别的层的节点是不同看的，这里全部用的深度遍历，其实定位倒数第二层时，用层序遍历也挺好，

```java
class Solution {
    //若是满二叉树的话节点个数是 2^n - 1
    //先遍历找到层数
    //然后遍历所有节点 如果有除了叶子节点的
    int sumOfMinus = 0;
    public int countNodes(TreeNode root) {
        int level = 0;
        if(root == null)
            return 0;
        //找到并记录层数的值
        else
            level = findLevel(root, 1);
        if(level == 1)
            return 1;
        else{
            findNodeToMinus(root, 1, level);
            return (int)Math.pow(2, level) - 1 - sumOfMinus;
        }
    }
    //找到并返回这个树的层数
    //由于是完全二叉树，所以只需要一路向左递归就知道了level的值
    public int findLevel(TreeNode root, int level){
        if(root == null)
            return level - 1;
        return findLevel(root.left, level + 1);
    }
    //在已知整个完全二叉树的层数的情况下，bound即边界设置为level的值
    //这样我们遍历到倒数第二层的时候，即当前的level == bound - 1时 进行判断
    //其它的情况不做任何的处理 继续进行递归就可以
    public void findNodeToMinus(TreeNode root, int level, int bound){
        if(root == null)
            return ;
        //找到了倒数第二层的节点
        if(level == bound - 1){
            //如果没有左节点 那就少了两个节点个数
            if(root.left == null)
                sumOfMinus += 2;
            //如果只是没有右节点，那就少了一个节点数
            else if(root.right == null)
                sumOfMinus += 1;
            return ;
        }
        findNodeToMinus(root.right, level + 1, bound);
        findNodeToMinus(root.left, level + 1, bound);
        return ;
    }
}
```

#### 897 递增顺序查找树

```markdown
给你一个树，请你 按中序遍历 重新排列树，使树中最左边的结点现在是树的根，并且每个结点没有左子结点，只有一个右子结点。

示例 ：

输入：[5,3,6,2,4,null,8,1,null,null,null,7,9]

       5
      / \
    3    6
   / \    \
  2   4    8
 /        / \ 
1        7   9

输出：[1,null,2,null,3,null,4,null,5,null,6,null,7,null,8,null,9]

 1
  \
   2
    \
     3
      \
       4
        \
         5
          \
           6
            \
             7
              \
               8
                \
                 9  
 
提示：

给定树中的结点数介于 1 和 100 之间。
每个结点都有一个从 0 到 1000 范围内的唯一整数值。
```

思路1：中序遍历 + 构造新的树

思路2：更改树的连接方式，拿一个cur来记录上次递归到的节点，然后在中序遍历的基础上更改节点的连接方式

```java
class Solution {
    TreeNode cur;
    public TreeNode increasingBST(TreeNode root) {
        if(root == null) return null;
        TreeNode node = new TreeNode(0);
        cur = node;
        recur(root);
        return node.right;
    }	
    public void recur(TreeNode root){
        if(root == null) return ;
        recur(root.left);
        root.left = null;
        cur.right = root;
        cur = root;
        recur(root.right);
    }
}
```

#### 501 二叉搜索树的众数

```markdown
给定一个有相同值的二叉搜索树（BST），找出 BST 中的所有众数（出现频率最高的元素）。

假定 BST 有如下定义：

结点左子树中所含结点的值小于等于当前结点的值
结点右子树中所含结点的值大于等于当前结点的值
左子树和右子树都是二叉搜索树
例如：
给定 BST [1,null,2,2],

   1
    \
     2
    /
   2
返回[2].

提示：如果众数超过1个，不需考虑输出顺序

进阶：你可以不使用额外的空间吗？（假设由递归产生的隐式调用栈的开销不被计算在内）
```

思路：二叉搜索树中序遍历是个递增的数，所以我们在递增的序列中找众数，用一个list来记录每个数的出现，然后再算众数

```java
//太笨的方法
```

#### 109 有序链表转化为二叉树

```markdown
给定一个单链表，其中的元素按升序排序，将其转换为高度平衡的二叉搜索树。

本题中，一个高度平衡二叉树是指一个二叉树每个节点 的左右两个子树的高度差的绝对值不超过 1。

示例:

给定的有序链表： [-10, -3, 0, 5, 9],

一个可能的答案是：[0, -3, 9, -10, null, 5], 它可以表示下面这个高度平衡二叉搜索树：

      0
     / \
   -3   9
   /   /
 -10  5
```

思路：对于每个区间，我们求该区间的mid，然后mid左递归制造左子树，mid右递归制造右子树

```java
class Solution {
    public TreeNode sortedListToBST(ListNode head) {
        if(head == null) return null;
        if(head.next == null) return head.val;
        
        List<Integer> list = new ArrayList<>();
        while(head != null){
            list.add(head.val);
            head = head.next;
        }
        return buildTree(0, list.size() - 1, list);
    }
    public TreeNode buildTree(int start, int end, List<Integer> list){
        if(start > end) return null;
        int mid = start + (end - start + 1) / 2;
        TreeNode root = new TreeNode(list.get(mid));
        root.left = buildTree(start, mid - 1, list);
        root.right = buildTree(mid + 1, end, list);
        return root;
    }
}
```

#### 96 不同的二叉搜索树

```markdown
给定一个整数 n，求以 1 ... n 为节点组成的二叉搜索树有多少种？

示例:

输入: 3
输出: 5
解释:
给定 n = 3, 一共有 5 种不同结构的二叉搜索树:

   1         3     3      2      1
    \       /     /      / \      \
     3     2     1      1   3      2
    /     /       \                 \
   2     1         2                 3
```

思路：递归，但是超时了，可以看出这里有好多重复计算，==其实只和区间的长度有关系==，所以设置一个HashMap来保存记录值

```java
class Solution {
    public int numTrees(int n) {
        if(n == 0)	return 0;
        return compute(1, n);
    }
    public int compute(int start, int end) {
        if(start >= end) return 1;
        int res = 0;
        for(int i = start; i <= end; i++){
            int left = compute(start, i - 1);
            int right = compute(i + 1, end);
            res += left * right;
        }
        return res;
	} 
}
```

```java
class Solution {
    HashMap<Integer, Integer> map = new HashMap<>();
    public int numTrees(int n) {
        if(n == 0)	return 0;
        return compute(1, n);
    }
    public int compute(int start, int end) {
        if(start >= end) return 1;
        int res = 0;
        for(int i = start; i <= end; i++){
            int left, right;
            if(!map.containsKey(i - start - 1)){
                left = compute(start, i - 1);
                map.put(i - start - 1, left);
            }
            else 
                left = map.get(i - start - 1);
            if(!map.containsKey(end - i - 1)){
                right = compute(i + 1, end);
                map.put(end - i - 1, right);
            }
            else 
                right = map.get(end - i - 1);
            res += left * right;
        }
        return res;
	} 
}
```

#### 114 二叉树展开为链表

```
给定一个二叉树，原地将它展开为一个单链表。

 

例如，给定二叉树

    1
   / \
  2   5
 / \   \
3   4   6
将其展开为：

1
 \
  2
   \
    3
     \
      4
       \
        5
         \
          6

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/flatten-binary-tree-to-linked-list
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
```

思路：==看了一个题解，反向的后续遍历，这个递归好漂亮，用反向后序遍历的原因是，方便保存要左子树要指向的节点==

```java
class Solution {
    public TreeNode pre;
    public void flatten(TreeNode root) {
        if(root == null)	return;
        flatten(root.right);
        flatten(root.left);
        
        root.left = null;
        root.right = pre;
        pre = root;
    }
}
```



##二分查找

#### 34 在排序数组中查找元素的第一个和最后一个位置

```markdown
给定一个按照升序排列的整数数组 nums，和一个目标值 target。找出给定目标值在数组中的开始位置和结束位置。

如果数组中不存在目标值 target，返回 [-1, -1]。

进阶：

你可以设计并实现时间复杂度为 O(log n) 的算法解决此问题吗？
 

示例 1：

输入：nums = [5,7,7,8,8,10], target = 8
输出：[3,4]
示例 2：

输入：nums = [5,7,7,8,8,10], target = 6
输出：[-1,-1]
示例 3：

输入：nums = [], target = 0
输出：[-1,-1]
 

提示：

0 <= nums.length <= 105
-109 <= nums[i] <= 109
nums 是一个非递减数组
-109 <= target <= 109
```

思路：由于是非递减数组，想到要用二分查找，我们第一个要找的值，是target第一出现的位置，第二个要找的值，其实是第一个比target大的位置 - 1，最后我们再判断这两个值找的对不对即可

剩下的是做二分查找，为了更好的复用代码，我们设置了一个lower值来进行标识此次找的是大于的还是大于等于的

```java
class Solution {
    public int[] searchRange(int[] nums, int target) {
        int leftIndex = binarySearch(nums, target, true);
        int rightIndex = binarySearch(nums, target, false) - 1;
        if(leftIndex <= rightIndex && rightIndex < nums.length && nums[leftIndex] == target && nums[rightIndex] == target)
            return new int []{leftIndex, rightIndex};
        return new int []{-1, -1};
    }
    public int binarySearch(int [] nums, int target, boolean lower){
        int left = 0, right = nums.length - 1, res = nums.length;
        while(left <= right){
            int mid = (left + right) / 2;
            if(nums[mid] > target || (lower && nums[mid] >= target)){
                right = mid - 1;
                res = mid;
            }else{
                left = mid + 1;
            }
        }
        return res;
    }
}
```

#### 74 搜索二维矩阵

```
编写一个高效的算法来判断 m x n 矩阵中，是否存在一个目标值。该矩阵具有如下特性：

每行中的整数从左到右按升序排列。
每行的第一个整数大于前一行的最后一个整数。
 

示例 1：


输入：matrix = [[1,3,5,7],[10,11,16,20],[23,30,34,50]], target = 3
输出：true
示例 2：


输入：matrix = [[1,3,5,7],[10,11,16,20],[23,30,34,50]], target = 13
输出：false
示例 3：

输入：matrix = [], target = 0
输出：false
 

提示：

m == matrix.length
n == matrix[i].length
0 <= m, n <= 100
-104 <= matrix[i][j], target <= 104

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/search-a-2d-matrix
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
```

思路：第一反应就是二分查找，从下标找到规律，我们虚拟出来一个一维数组，对于在这个一维数组中的索引位置idx

​			对于二维矩阵中满足 i = idx / n   j = idx % n

​			剩下的就是正常的二分查找了

```java
class Solution {
    public boolean searchMatrix(int[][] matrix, int target) {
        //二分查找，我们虚拟出来一个一维数组，对于在这个一维数组中的索引位置idx
        //对于二维矩阵中满足 i = idx / n   j = idx % n
        int row = matrix.length;
        if(row == 0)    return false;
        int col = matrix[0].length;
        int left = 0;
        int right = row * col - 1;
        while(left <= right){
            int middle = (left + right) / 2;
            int num = matrix[middle / col][middle % col];
            if(num > target){
                right = middle - 1;
            }
            else if(num < target){
                left = middle + 1;
            }else{
                return true;
            }
        }
        return false;
    }
}
```

## 链表

#### 61 旋转链表

```markdown
给定一个链表，旋转链表，将链表每个节点向右移动 k 个位置，其中 k 是非负数。

示例 1:

输入: 1->2->3->4->5->NULL, k = 2
输出: 4->5->1->2->3->NULL
解释:
向右旋转 1 步: 5->1->2->3->4->NULL
向右旋转 2 步: 4->5->1->2->3->NULL
示例 2:

输入: 0->1->2->NULL, k = 4
输出: 2->0->1->NULL
解释:
向右旋转 1 步: 2->0->1->NULL
向右旋转 2 步: 1->2->0->NULL
向右旋转 3 步: 0->1->2->NULL
向右旋转 4 步: 2->0->1->NULL
```

思路：双指针 一个先往前走k % length - 1的位置，另一个就在原地不动，然后两者同时往后走，当第一个指针走到.next == null时，第二个指针的位置正好就是要当头节点的位置,然后再设置第三个指针指向第二个指针的前一个,这样

```java
class Solution {
    public ListNode rotateRight(ListNode head, int k) {
		if(head == null) return null;
        if(head.next == null || k == 0) return head;
        ListNode p = head, q = head, m = head;
        int length = 0;
        while(p != null){
            length ++;
            p = p.next;
        }
        p = head;
        k = k  % length - 1;
        if(k == -1) return head;
        while(k != 0){
            k --;
            p = p.next;
        }
        boolean first = true;
        while(p.next != null){
            if(! first) m = m.next;
            else first = false;
            p = p.next;
            q = q.next;
        }
        m.next = null;
        p.next = head;
        return q;
    }
}
```

#### 1171 从链表中删去总值为零的连续节点

```markdown
给你一个链表的头节点 head，请你编写代码，反复删去链表中由 总和 值为 0 的连续节点组成的序列，直到不存在这样的序列为止。

删除完毕后，请你返回最终结果链表的头节点。

你可以返回任何满足题目要求的答案。

（注意，下面示例中的所有序列，都是对 ListNode 对象序列化的表示。）

示例 1：

输入：head = [1,2,-3,3,1]
输出：[3,1]
提示：答案 [1,2,1] 也是正确的。
示例 2：

输入：head = [1,2,3,-3,4]
输出：[1,2,4]
示例 3：

输入：head = [1,2,3,-3,-2]
输出：[1]

提示：
给你的链表中可能有 1 到 1000 个节点。
对于链表中的每个节点，节点的值：-1000 <= node.val <= 1000.
```

思路：双重循环，从每个节点看是否从这个节点开始使得之后的某一段数的和为0，那么就去掉这段数，最后跑到末尾，继续遍历下一个节点，直到这个链中所有的节点遍历完为止

```java
class Solution {
    public ListNode removeZeroSumSublists(ListNode head) {
		ListNode p = new ListNode(0);
        ListNode pre = p;
        p.next = head;
        while(p != null){
            ListNode cur = p.next;
            int sum = 0;
            while(cur != null){
                sum += cur.val;
                cur = cur.next;
                if(sum == 0){
                    p.next = cur;
                    break;
                }
            }
            if(cur == null) p = p.next;
        }
        return pre.next;
    }
}
```

#### 206 反转链表

```
206. 反转链表
反转一个单链表。

示例:

输入: 1->2->3->4->5->NULL
输出: 5->4->3->2->1->NULL
进阶:
你可以迭代或递归地反转链表。你能否用两种方法解决这道题？
```

思路：类似于深度遍历，先一步步递归到最后一个节点，然后再倒数第二个节点和最后一个节点之间改变指针方向，注意我们一直返回的p实际上就是最后一个节点，即反转后的头节点

这个p节点的设置比较巧妙

```java
class Solution {
    public ListNode reverseList(ListNode head) {
		if(head == null || head.next == null)	return head;
        ListNode p = reverseList(head.next);
        head.next.next = head;
        head.next = null;
        return p;
    }
}
```

#### 92 反转链表2

```
反转从位置 m 到 n 的链表。请使用一趟扫描完成反转。

说明:
1 ≤ m ≤ n ≤ 链表长度。

示例:

输入: 1->2->3->4->5->NULL, m = 2, n = 4
输出: 1->4->3->2->5->NULL
```

思路：==我们先来实现一个函数，反转链表前n个数==，

```java
//有一个标记在第n个节点之后的这个节点的全局变量，初始化为null
public ListNode nextNode = null;
public ListNode reverseList(ListNode head, int n) {
	if(n == 1){
        nextNode = head.next;
        return head;
    }
    ListNode p = reverseList(head.next, n - 1);
    head.next.next = head;
    //各个节点的next在递归过程中不断指向nextNode，然后在返回上一层的递归时（除了之前的第一个节点以外）这个指针又被修改
    head.next = nextNode;
    return p;
}
```

==然后我们对于区间[m , n]的进行反转，可以看出，如果m = 1时，我们要做的就是上面那个函数的工作，那么如果m不是1==

==我们应该怎么做，实际上就是往后next，一下，然后相对的区间就变成了[m - 1, n - 1],直到m变成1为止==

**太强了，对于递归的理解真滴好！**

```java
class Solution {
    public ListNode nextNode = null;
    public ListNode reverseBetween(ListNode head, int m, int n) {
   		if(m == 1)
            return reverseList(head, n);
        else{
            head.next = reverseBetween(head.next, m - 1, n - 1);
        	return head;
        }
    }  
    public ListNode reverseList(ListNode head, int n) {
        if(n == 1){
            nextNode = head.next;
            return head;
    	}
    	ListNode p = reverseList(head.next, n - 1);
    	head.next.next = head;
    	//各个节点的next在递归过程中不断指向nextNode，然后在返回上一层的递归时（除了之前的第一个节点以外）这个指针又被修改
   	 	head.next = nextNode;
    	return p;
	}
}
```

#### 21 合并两个有序链表

```
将两个升序链表合并为一个新的 升序 链表并返回。新链表是通过拼接给定的两个链表的所有节点组成的。 

示例：

输入：1->2->4, 1->3->4
输出：1->1->2->3->4->4
```

思路1：迭代思路，额外的链表空间，设置一个头指针不断地指向l1和l2中较小的那个节点，即直接在两个链表之间跳动找最小的，并且指向它

```java
class Solution {
    public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        if(l1 == null) return l2;
        if(l2 == null) return l1;
        ListNode preHead = new ListNode(0);
        preHead.next = l1.val > l2.val ? l2 : l1;
        ListNode p = preHead.next;
        if(p == l2)	
            l2 = l2.next;
        else
            l1 = l1.next;
        while(l1 != null && l2 != null){
            p.next = l1.val > l2.val ? l2 : l1;
            if(p.next == l2) 
                l2 = l2.next;
            else	
                l1 = l1.next;
            p = p.next;
        }
        if(l1 == null)
            p.next = l2;
        else 
            p.next = l1;
        return preHead.next;
    }
}
```

思路2：递归,return该return的头节点即可

```java
class Solution {
    public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        if(l1 == null) 	return l2;
        if(l2 == null)	return l1;
        if(l1.val < l2.val){
            l1.next = mergeTwoLists(l1.next, l2);
            return l1;
        }
        else{
            l2.next = mergeTwoLists(l1, l2.next);
        	return l2;
        }
    }
}
```

#### 24 两两交换链表中的节点

```
给定一个链表，两两交换其中相邻的节点，并返回交换后的链表。

你不能只是单纯的改变节点内部的值，而是需要实际的进行节点交换。

示例 1：

输入：head = [1,2,3,4]
输出：[2,1,4,3]
示例 2：

输入：head = []
输出：[]
示例 3：

输入：head = [1]
输出：[1]
提示：

链表中节点的数目在范围 [0, 100] 内
0 <= Node.val <= 100
```

思路：画了个图一次AC的，递归好爽！

```java
class Solution {
    public ListNode swapPairs(ListNode head) {
        if(head == null || head.next == null) return head;
        ListNode p = head.next;
        head.next = swapPairs(p.next);
        p.next = head;
        return p;
    }
}
```

#### 82 删除排序链表的重复元素2

```
给定一个排序链表，删除所有含有重复数字的节点，只保留原始链表中 没有重复出现 的数字。

示例 1:

输入: 1->2->3->3->4->4->5
输出: 1->2->5
示例 2:

输入: 1->1->1->2->3
输出: 2->3
```

思路1：迭代 + 双指针

```java
class Solution {
    public ListNode deleteDuplicates(ListNode head) {
		if(head == null)	return null;
        ListNode preHead = new ListNode(0);
        preHead.next = head;
        ListNode p = preHead;
        ListNode q = head;
        while(q != null && q.next != null){
            if(p.next.val != q.next.val){
                p = p.next;
                q = q.next;
            }else{
                while(q != null && q.next != null && p.next.val == q.next.val){
                    q = q.next;
                }
                p.next = q.next;
                q = q.next;
            }
        }
        return preHead.next;
    }
}
```

思路2：==递归==，自己没想出来

```java
class Solution {
    public ListNode deleteDuplicates(ListNode head) {
		if(head == null || head.next == null)	return head;
        if(head.next.val == head.val){
            while(head.val == head.next.val)
            	head = head.next;
        	return deleteDuplicates(head.next);
        }else{
            head.next = deleteDuplicates(head.next);
            return head;
        }
    }
}
```

#### 面试题02.05 链表求和 【两数相加】

```
给定两个用链表表示的整数，每个节点包含一个数位。

这些数位是反向存放的，也就是个位排在链表首部。

编写函数对这两个整数求和，并用链表形式返回结果。

示例：

输入：(7 -> 1 -> 6) + (5 -> 9 -> 2)，即617 + 295
输出：2 -> 1 -> 9，即912
进阶：思考一下，假设这些数位是正向存放的，又该如何解决呢?

示例：

输入：(6 -> 1 -> 7) + (2 -> 9 -> 5)，即617 + 295
输出：9 -> 1 -> 2，即912

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/sum-lists-lcci
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
```

思路：迭代，每次计算时注意一个carry值，即标记有没有进位

```java
class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
		if(l1 == null)	return l2;
        if(l2 == null)	return l1;
        ListNode preHead = new ListNode(0);
        ListNode currentNode = preHead;
        int carry = 0;
        while(l1 != null || l2 != null){
            int x = l1 == null ? 0 : l1.val;
            int y = l2 == null ? 0 : l2.val;
            int sum = x + y + carry;
            carry = sum / 10;
            sum %= 10;
            currentNode.next = new ListNode(sum);
            currentNode = currentNode.next;
            if(l1 != null)	l1 = l1.next;
            if(l2 != null)	l2 = l2.next;
        }
        //最后计算一下是否有进位
        if(carry == 1)	currentNode.next = new ListNode(1);
        return preHead.next;
    }
}
```

#### 328 奇偶链表

```
给定一个单链表，把所有的奇数节点和偶数节点分别排在一起。请注意，这里的奇数节点和偶数节点指的是节点编号的奇偶性，而不是节点的值的奇偶性。

请尝试使用原地算法完成。你的算法的空间复杂度应为 O(1)，时间复杂度应为 O(nodes)，nodes 为节点总数。

示例 1:

输入: 1->2->3->4->5->NULL
输出: 1->3->5->2->4->NULL
示例 2:

输入: 2->1->3->5->6->4->7->NULL 
输出: 2->3->6->7->1->5->4->NULL
说明:

应当保持奇数节点和偶数节点的相对顺序。
链表的第一个节点视为奇数节点，第二个节点视为偶数节点，以此类推。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/odd-even-linked-list
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
```

思路：一直想着怎么用递归做，发现迭代其实更好想，我们先链出所有奇数链，再链出所有偶数链，最后将偶数链拼接在奇数链上,注意实际上做的时候不能两个链表先生成一个，再生成一个，应该是交叉生成，因为链表的指针在分开时会变化

```java
class Solution {
    public ListNode oddEvenList(ListNode head) {
        if(head == null || head.next == null)	return head;
        ListNode preHead = new ListNode(0);
        preHead.next = head;
        ListNode odd = head;
        ListNode even = head.next;
        ListNode firstEven = even;
        while(even != null && even.next != null){
            odd.next = even.next;
            odd = odd.next;
            even.next = odd.next;
            even = even.next;
        }
        odd.next = firstEven;
        return preHead.next;
    }
}
```

#### 445 两数相加2

```
给你两个 非空 链表来代表两个非负整数。数字最高位位于链表开始位置。它们的每个节点只存储一位数字。将这两数相加会返回一个新的链表。

你可以假设除了数字 0 之外，这两个数字都不会以零开头。

进阶：

如果输入链表不能修改该如何处理？换句话说，你不能对列表中的节点进行翻转。

示例：

输入：(7 -> 2 -> 4 -> 3) + (5 -> 6 -> 4)
输出：7 -> 8 -> 0 -> 7

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/add-two-numbers-ii
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
```

==❌没看清题目，我以为是从左向右加，用下面的写出来的是，7708❌，大意了==

```java
class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        if(l1 == null)	return l2;
        if(l2 == null)	return l1;
        int length1 = length(l1);
        int length2 = length(l2);
        ListNode p1 = lenght1 > length2 ? l1 : l2;
        ListNode preHead = new ListNode(0);
        preHead.next = p1;
        ListNode p2 = length1 > lenght2 ? l2 : l1;
       	int d = Math.abs(length1 - length2);
        while(d > 0){
            p1 = p1.next;
            d -- ;
        }
        int carry = 0;
        ListNode preP1 = new ListNode(0);
        preP1.next = p1;
        boolean b = true;
        while(p1 != null){
            int x = p1.val;
            int y = p2.val;
            int sum = x + y + carry;
            carry = sum / 10;
            sum %= 10;
            p1.val = sum;
            if(b){
                b = false;
            }else{
                preP1 = preP1.next;
            }
            p1 = p1.next;
            p2 = p2.next;
        }
        if(carry != 0)
            preP1.next = new ListNode(1);
        return preHead.next;
    }
    public int length(ListNode k){
        if(k == null)	return 0;
        return 1 + lenght(k.next);
    }
}
```

思路：可以看到和上面一个两数相加的区别是顺序不同，因此我们可以用栈存储它们的值，然后相加即可，需要注意的是头节点的位置是不断更新的,最后输出

```java
class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        Deque<Integer> stack1 = new LinkedList<Integer>();
        Deque<Integer> stack2 = new LinkedList<Integer>();
    	while(l1 != null){
            stack1.push(l1.val);
            l1 = l1.next;
        }
        while(l2 != null){
            stack2.push(l2.val);
            l2 = l2.next;
        }
        int carry = 0;
        ListNode res = null;
        while(!stack1.isEmpty() || !stack2.isEmpty() || carry != 0){
            int x = stack1.isEmpty() ? 0 : stack1.pop();
            int y = stack2.isEmpty() ? 0 : stack2.pop();
        	int sum = x + y + carry;
            carry = sum / 10;
            sum %= 10;
            ListNode cur = new ListNode(sum);
            //注意这里！
            cur.next = res;
            res = cur;
        }
        return res;
    }
}
```

## 回溯

### 总结

参考https://leetcode-cn.com/problems/permutations/solution/hui-su-suan-fa-python-dai-ma-java-dai-ma-by-liweiw/（这篇题解写的特别好）

- 回溯算法可以搜索得到所有的方案（当然包括最优解），但是本质上它是一种遍历算法，时间复杂度很高。

- 执行深度优先遍历，从较深层的结点返回到较浅层结点的时候，需要做「状态重置」，即「回到过去」、「恢复现场」。

- 剪枝：回溯算法会应用「剪枝」技巧达到以加快搜索速度。有些时候，需要做一些预处理工作（例如排序）才能达到剪枝的目的。预处理工作虽然也消耗时间，但能够剪枝节约的时间更多；
  提示：剪枝是一种技巧，通常需要根据不同问题场景采用不同的剪枝策略，需要在做题的过程中不断总结。

  由于回溯问题本身时间复杂度就很高，所以能用空间换时间就尽量使用空间

- **做题的时候，建议 先画树形图 ，画图能帮助我们想清楚递归结构，想清楚如何剪枝。拿题目中的示例，想一想人是怎么做的，一般这样下来，这棵递归树都不难画出。在画图的过程中思考清楚：**

  分支如何产生？
  题目需要的解在哪里？是在叶子结点、还是在非叶子结点、还是在从跟结点到叶子结点的路径？
  哪些搜索会产生不需要的解的？例如：产生重复是什么原因，如果在浅层就知道这个分支不能产生需要的结果，应该提前剪枝，剪枝的条件是什么，代码怎么写？

**可以简单的分类为：排列、组合、子集相关问题     Flood Fill    字符串中的回溯问题      游戏问题**

#### 39 组合总和

```
给定一个无重复元素的数组 candidates 和一个目标数 target ，找出 candidates 中所有可以使数字和为 target 的组合。

candidates 中的数字可以无限制重复被选取。

说明：

所有数字（包括 target）都是正整数。
解集不能包含重复的组合。 
示例 1：

输入：candidates = [2,3,6,7], target = 7,
所求解集为：
[
  [7],
  [2,2,3]
]
示例 2：

输入：candidates = [2,3,5], target = 8,
所求解集为：
[
  [2,2,2,2],
  [2,3,3],
  [3,5]
]
 

提示：

1 <= candidates.length <= 30
1 <= candidates[i] <= 200
candidate 中的每个元素都是独一无二的。
1 <= target <= 500
通过次数186,545提交次数2

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/combination-sum
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
```

思路：==//自己第一次写出来的存在重复元组//[[2,2,3],[2,3,2],[3,2,2],[7]]==，这道题怎么去重（剪枝）是关键，也是难点，代码中设置了一个==begin==，来标记下次递归回溯时应该从哪个元素开始，这样就可以规避重复元组。元素可以重复使用，所以没必要使用used数组。

- 但其实这个写法还是有剪枝的空间的，因为我们可以对于一个排序数组进行剪枝，当前的值大于target时，这个for循环后面的数就不用计算了。

```java
class Solution {
    int target;
    boolean[] used;
    int k = 0;
    //自己写出来的存在重复元组
    //[[2,2,3],[2,3,2],[3,2,2],[7]]
    //即需要剪枝
    public List<List<Integer>> combinationSum(int[] candidates, int target) {
        List<List<Integer>> resList = new ArrayList<>();
        int len = candidates.length;
        if(len == 0)    return resList;
        //判断条件应该是现在的sum是否大于target
        List<Integer> path = new ArrayList<>();
        this.target = target;
        dfs(path, 0,resList, candidates, 0, len);
        return resList;
    }
    public void dfs(List<Integer> path, int begin,List<List<Integer>> resList, int[] candidates, int sum, int len){
        if(sum > target)
            return;
        if(sum == target){
            resList.add(new ArrayList<>(path));
            return;
        }
        for(int i = begin;i < len;i++){
        	sum += candidates[i];
            path.add(candidates[i]);

            dfs(path, i, resList, candidates, sum, len);

            sum -= candidates[i];
            path.remove(path.size() - 1);    
        } 
        
    }
}
```

- 再剪枝

```jaVa

```

#### 46 全排列

```
给定一个 没有重复 数字的序列，返回其所有可能的全排列。

示例:

输入: [1,2,3]
输出:
[
  [1,2,3],
  [1,3,2],
  [2,1,3],
  [2,3,1],
  [3,1,2],
  [3,2,1]
]

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/permutations
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
```

思路：==**要十分注意一点：resList中存放的其实是地址，如果使用的是，resList.add(path)，在递归过程中，path确实是加入了resList，但由于java中这个保存的是地址，在递归结束时，该地址所指向的list其实是空的，即最终的结果将是几个空的list。所以这里应该用一个复制出来的list进行存储**==

```java
class Solution {
    public List<List<Integer>> permute(int[] nums) {
        List<List<Integer>> resList = new ArrayList<>();
        int len = nums.length;
        if(len == 0)    return resList;
        boolean used[] = new boolean[len];
        List<Integer> path = new ArrayList<>();
        dfs(resList, path, len, 0, used, nums);
        return resList;
    }
    public void dfs(List<List<Integer>> resList, List<Integer> path, int len, int depth, boolean[] used, int[] nums){
        if(depth == len){
            resList.add(new ArrayList<>(path));
            return ;
        }
        for(int i = 0;i < len;i++){
            if(!used[i]){
                path.add(nums[i]);
                used[i] = true;

                dfs(resList, path, len, depth + 1, used, nums);

                used[i] = false;
                path.remove(path.size() - 1);
            }
        }
    }
}
```

#### 47 全排列2

```
给定一个可包含重复数字的序列 nums ，按任意顺序 返回所有不重复的全排列。

示例 1：

输入：nums = [1,1,2]
输出：
[[1,1,2],
 [1,2,1],
 [2,1,1]]
示例 2：

输入：nums = [1,2,3]
输出：[[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]
 
提示：

1 <= nums.length <= 8
```

思路：与上题唯一不同的是，这个题需要去重复，如果在最后的结果中进行去重复，则变得比较困难和难处理，我们考虑在递归过程中进行限制，==首先我们将nums进行排序，这是我们剪枝的前提（常用的技巧），然后我们跳过所有满足nums[i] == nums[i - 1]，因为这个在上一个兄弟分支已经执行过了==

```java
class Solution {
    public List<List<Integer>> permuteUnique(int[] nums) {
        List<List<Integer>> resList = new ArrayList<>();
        int len = nums.length;
        if(len == 0)    return resList;
        boolean used[] = new boolean[len];
        Arrays.sort(nums);
        List<Integer> path = new ArrayList<>();
        dfs(resList, path, len, 0, used, nums);
        return resList;
    }
    public void dfs(List<List<Integer>> resList, List<Integer> path, int len, int depth, boolean[] used, int[] nums){
        if(depth == len){
            resList.add(new ArrayList<>(path));
            return ;
        }
        for(int i = 0;i < len;i++){
            //!used[i - 1]是因为在dfs过程中，刚刚被撤销了used
            if(i > 0 && nums[i] == nums[i - 1] && !used[i - 1])
                continue;

            if(!used[i]){
                path.add(nums[i]);
                used[i] = true;

                dfs(resList, path, len, depth + 1, used, nums);

                used[i] = false;
                path.remove(path.size() - 1);
            }
        }
    }
}
```

#### 77 全排列

```
给定两个整数 n 和 k，返回 1 ... n 中所有可能的 k 个数的组合。

示例:

输入: n = 4, k = 2
输出:
[
  [2,4],
  [3,4],
  [2,3],
  [1,2],
  [1,3],
  [1,4],
]
```

思路：根据题意，我们用一个begin来进行剪枝,注意我们下一次的begin值的变化

- 但是其实你发现，对于这个树还是能够进行剪枝，我们对于n = 7 , k = 4，**从5开始就没有意义了**，因为后面的数5 6 7 不够四个，即从五开始压根就不用做了。**即这个问题有搜索起点的上界**。更加一般化的，如果现在的path的大小为size，那么对于k - size，还有意义，对于大于等于n - (k - size) + 1的后面的数，我们没有必要搜索了。

```java
class Solution {
    public int k;
    public List<List<Integer>> combine(int n, int k) {
		List<List<Integer>> resList = new ArrayList<>();
        if(k == 0)	return resList;
        this.k = k;
        //为了方便，我们使用一个数组
        int[] nums = new int[n];
        for(int i = 0;i < n;i++)	nums[i] = i + 1;
        //使用Deque 是官方 使用stack数据结构的建议
        Deque<Integer> path = new ArrayDeque<>();
        dfs(path, resList, nums, 0, 0);
        return resList;
    }
    public void dfs(Deque<Integer> path, List<List<Integer>> resList, int[] nums, int begin, int depth){
        if(depth == k){
            resList.add(new ArrayList<>(path));
            return ;
        }
        for(int i = begin;i < nums.length;i++){
            path.add(nums[i]);
            
            dfs(path, resList, nums, i + 1, depth + 1);
            
            path.removeLast();
        }
    }
}
```

改进后：

```java
class Solution {
    public int k;
    public List<List<Integer>> combine(int n, int k) {
		List<List<Integer>> resList = new ArrayList<>();
        if(k == 0)	return resList;
        this.k = k;
        //为了方便，我们使用一个数组
        int[] nums = new int[n];
        for(int i = 0;i < n;i++)	nums[i] = i + 1;
        //使用Deque 是官方 使用stack数据结构的建议
        Deque<Integer> path = new ArrayDeque<>();
        dfs(path, resList, nums, 0, 0);
        return resList;
    }
    public void dfs(Deque<Integer> path, List<List<Integer>> resList, int[] nums, int begin, int depth){
        if(depth == k){
            resList.add(new ArrayList<>(path));
            return ;
        }
        for(int i = begin;i < nums.length - (k - path.size()) + 1;i++){
            path.add(nums[i]);
            
            dfs(path, resList, nums, i + 1, depth + 1);
            
            path.removeLast();
        }
    }
}
```

#### 78 子集

```
给定一组不含重复元素的整数数组 nums，返回该数组所有可能的子集（幂集）。

说明：解集不能包含重复的子集。

示例:

输入: nums = [1,2,3]
输出:
[
  [3],
  [1],
  [2],
  [1,2,3],
  [1,3],
  [2,3],
  [1,2],
  []
]

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/subsets
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
```

思路：用begin为i+1 来限制下一层递归的起始位置在哪儿

```java
class Solution {
    public List<List<Integer>> subsets(int[] nums) {
		List<List<Integer>> resList = new ArrayList<>();
        int length = nums.length;
        Deque<Integer> path = new ArrayDeque<>();
        resList.add(new ArrayList<>(path));
        if(length == 0)	return resList;
        dfs(nums, resList, 0, length, path, 0);
        return resList;
    }
    public void dfs(int[] nums, List<List<Integer>> resList, int begin, int length,  Deque<Integer> path, int depth){
        if(depth == length){
            return;
        }
        for(int i = begin;i < length;i++){
            path.add(nums[i]);
            resList.add(new ArrayList<>(path));
            dfs(nums, resList, i + 1, length, path, depth + 1);
            path.removeLast();
        }
    }
}
```

#### 90 子集2

```
给定一个可能包含重复元素的整数数组 nums，返回该数组所有可能的子集（幂集）。

说明：解集不能包含重复的子集。

示例:

输入: [1,2,2]
输出:
[
  [2],
  [1],
  [1,2,2],
  [2,2],
  [1,2],
  []
]

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/subsets-ii
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
```

思路：类似于全排列2的做法,==但是效率有点低==，应该还可以继续剪枝

```java
class Solution {
    public boolean[] used;
    public List<List<Integer>> subsetsWithDup(int[] nums) {
		//记得要排序
        List<List<Integer>> resList = new ArrayList<>();
        int length = nums.length;
        Deque<Integer> path = new ArrayDeque<>();
        resList.add(new ArrayList<>(path));
        if(length == 0)	return resList;
        Arrays.sort(nums);
        used = new boolean[length];
        dfs(nums, resList, 0, length, path, 0);
        return resList;
    }
    public void dfs(int[] nums, List<List<Integer>> resList,int begin, int length,  Deque<Integer> path, int depth){
        if(depth == length){
            return;
        }
        for(int i = begin;i < length;i++){
            if(i > 0 && nums[i] == nums[i - 1] && !used[i - 1])
                continue;
            if(!used[i]){
                path.add(nums[i]);
                used[i] = true;
                resList.add(new ArrayList<>(path));
                dfs(nums, resList, i, length, path, depth + 1);
                path.removeLast();
                used[i] = false;
            }
        }
    }
}
```

#### 60 排序序列

```
给出集合 [1,2,3,...,n]，其所有元素共有 n! 种排列。

按大小顺序列出所有排列情况，并一一标记，当 n = 3 时, 所有排列如下：

"123"
"132"
"213"
"231"
"312"
"321"
给定 n 和 k，返回第 k 个排列。

示例 1：

输入：n = 3, k = 3
输出："213"
示例 2：

输入：n = 4, k = 9
输出："2314"
示例 3：

输入：n = 3, k = 1
输出："123"
 
提示：

1 <= n <= 9
1 <= k <= n!

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/permutation-sequence
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
```

思路：n个数共有n！的排列方式，那么对于n - 1个数应该是（n-1）!中排列方式，我们可以用这个数作为区间长度，来进行剪枝，就是确定了第一个要加入的值之后，然后再在其他的值上正常的进行回溯，然后在回溯结果生成的list中寻找结果，这个方法在边界情况（即我找的数恰巧是某个划分过的区间的最后一个排列）时，也很麻烦。总之，这个还是太inefficient。抽时间看看别人的题解。

（猜测可以优化的点：1.可以对于每个小于n的数都计算阶乘，然后一步步将区间缩小，而不只是对第一层做剪枝2.resList的使用花费了大量额外的空间）

​								`==锤子的，第一次完全自己写出来一个hard题目，虽然时间和空间都菜的离谱，还是值得肯定，有点进步==`

![image-20201209162704675](https://gitee.com/f0rest9999/images/raw/master/20201209162704.png)

```java
class Solution {
    public boolean used[];
    public int reminder;
    public String getPermutation(int n, int k) {
        int[] nums = new int [n];
        for(int i = 0;i < n;i++)
            nums[i] = i + 1;
        used = new boolean[n];
        int length = factorial(n - 1);
        List<String> resList = new LinkedList<>();
        StringBuffer path = new StringBuffer();
      	int beginIndex = k / length;
        int reminder = k % length;
        String res = null;
        if(reminder != 0){
            used[beginIndex] = true;
            path.append(nums[beginIndex]);
            dfs(nums, beginIndex, 1, n, resList, path);
            res = resList.get(reminder - 1);
        }
        else{
            used[beginIndex - 1] = true;
            path.append(nums[beginIndex - 1]);
            dfs(nums, beginIndex - 1, 1, n, resList, path);
            res = resList.get(resList.size() - 1);
        }
        
        return res;
    }
    
    public void dfs(int nums[], int beginIndex, int depth, int n, List<String> resList, StringBuffer path){
        if(depth == n){
            resList.add(new String(path));
        }
        for(int i = 0;i < nums.length;i++){
           	if(!used[i]){
            	path.append(nums[i]);
                used[i] = true;
                
                dfs(nums, beginIndex, depth + 1, n, resList, path);
                
                path.deleteCharAt(path.length() - 1);
                used[i] = false;
            }
        }
    }
    
    public int factorial(int num){
        int res = 1;
        while(num >= 1){
            res *= num;
            num --;
        }
        return res;
    }
}
```

思路2：（参考他人题解）==太强了，简洁清晰==

![image-20201209190454059](https://gitee.com/f0rest9999/images/raw/master/20201209190501.png)



```java
class Solution {
    public boolean[] used;
    public int[] f;
    public int n;
    public int k;
    public String getPermutation(int n, int k) {
    	this.n = n;
        this.k = k;
        f = new int [n + 1];
        f[0] = 1;
        for(int i = 1;i <= n;i++){
            f[i] = f[i - 1] * i;
        }
        used = new boolean[n + 1];
        StringBuilder path = new StringBuilder();
        dfs(0, path);
        return path.toString();
    }
    //index 表示在这一步还有几个数字未被选中
    public void dfs(int index, StringBuilder path){
        if(index == n)	return;
        
        //count 表示在该层，除了第一个数字，其他数字能构成的全排列数的个数
        int count = f[n - index - 1];
        //接下来的工作就是确认第一个数字到底是那个数字
        
        for(int i = 1;i <= n;i++){
            
            if(used[i])
                continue;
           	
            if(count < k){
                k -= count;
                continue;
            }
            //到这步为止，该递归已经确定了目前要添加的第一个未确定的数字是几，就是i的数值大小，即这个循环到k <= count 走了多少次
            path.append(i);
            used[i] = true;
            // 不用回溯（重置变量），算法设计是「一下子来到叶子结点」，没有回头的过程，即我们一下子就定位到到底要找的哪个分支了
            // 后面的数没有必要遍历去尝试了，直接return
            dfs(index + 1, path);
            return ;
        }
        
    }
}
```

![image-20201209193203250](https://gitee.com/f0rest9999/images/raw/master/20201209193203.png)

#### 93 复原IP地址

```
给定一个只包含数字的字符串，复原它并返回所有可能的 IP 地址格式。

有效的 IP 地址 正好由四个整数（每个整数位于 0 到 255 之间组成，且不能含有前导 0），整数之间用 '.' 分隔。

例如："0.1.2.201" 和 "192.168.1.1" 是 有效的 IP 地址，但是 "0.011.255.245"、"192.168.1.312" 和 "192.168@1.1" 是 无效的 IP 地址。
示例 1：

输入：s = "25525511135"
输出：["255.255.11.135","255.255.111.35"]
示例 2：

输入：s = "0000"
输出：["0.0.0.0"]
示例 3：

输入：s = "1111"
输出：["1.1.1.1"]
示例 4：

输入：s = "010010"
输出：["0.10.0.10","0.100.1.0"]
示例 5：

输入：s = "101023"
输出：["1.0.10.23","1.0.102.3","10.1.0.23","10.10.2.3","101.0.2.3"]
 
提示：

0 <= s.length <= 3000
s 仅由数字组成

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/restore-ip-addresses
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
```

思路：==参考题解，主要有以下四个注意点==：

- 一开始字符串的长度小于4或者是大于12的一定不能得到结果，这个结论可以一般化到中间结果的判断，产生剪枝行为
- 每个结点最多有三种截取方式：截1位、截2位、截3位，即每个节点最多有三个子节点
- 由于ip字段最多就4段，因此这个递归树最多就四层
- 每一个节点需要两个额外的状态来表示：`spiltTimes` 已经分隔出多少个ip字段、`begin`截取ip段的起始位置

```java
class Solution {
    public List<String> restoreIpAddresses(String s) {
		List<String> resList = new ArrayList<>();
        int len = s.length();
        if(len > 12 || len < 4)	return resList;
        Deque<String> path = new ArrayDeque<>(4);
        int spiltTimes = 0;
        dfs(s, resList, path, 0, spiltTimes, len);
        return resList;
    }
    
    public void dfs(String s, List<String> resList, Deque<String> path, int begin, int spiltTimes, int len){
        if(begin == len){
            if(spiltTimes == 4)
                //用“.”来划分path
                resList.add(String.join(".", path));
            return ;
        }
        //len - begin 表示剩余的还未分割的字符串的长度
        //分别对应分割一个和分割三个的边界
        if(len - begin < (4 - spiltTimes) || len - begin > (4 - spiltTimes) * 3)
    		return ;
        for(int i = 0;i < 3;i++){
            if(begin + i >= len)	break;
            int num = judge(s, begin, begin + i);
            if(num != -1){
                path.addLast(Integer.toString(num));
 				dfs(s, resList, path, begin + i + 1, spiltTimes + 1, len);
                path.removeLast();
            }
        }
    }
    public int judge(String s, int left, int right){
        int length = right - left + 1;
        if(length > 1 && s.charAt(left) == '0')
            return -1;
       	int res = 0;
        for(int i = left;i <= right;i++){
            res = res * 10 + s.charAt(i) - '0';
        }
        if(res > 255)
           	return -1;
        return res;
    }
}
```

#### 200 岛屿数量

```
给你一个由 '1'（陆地）和 '0'（水）组成的的二维网格，请你计算网格中岛屿的数量。

岛屿总是被水包围，并且每座岛屿只能由水平方向和/或竖直方向上相邻的陆地连接形成。

此外，你可以假设该网格的四条边均被水包围。

示例 1：

输入：grid = [
  ["1","1","1","1","0"],
  ["1","1","0","1","0"],
  ["1","1","0","0","0"],
  ["0","0","0","0","0"]
]
输出：1
示例 2：

输入：grid = [
  ["1","1","0","0","0"],
  ["1","1","0","0","0"],
  ["0","0","1","0","0"],
  ["0","0","0","1","1"]
]
输出：3
提示：

m == grid.length
n == grid[i].length
1 <= m, n <= 300
grid[i][j] 的值为 '0' 或 '1'

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/number-of-islands
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
```

要点：FLOOD FILL

​			(https://leetcode-cn.com/problems/number-of-islands/solution/dfs-bfs-bing-cha-ji-python-dai-ma-java-dai-ma-by-l/)

![image-20201213130557744](https://gitee.com/f0rest9999/images/raw/master/20201213130557.png)

**这类的题目是有一定的套路的，怎么设置方向数组以及visited，多看多练就会了**

思路1：深度优先遍历

```java
class Solution {
    private char[][] grid;
    private boolean[][] visited;
    private int[][] directions = {{-1, 0}, {1, 0}, {0, 1}, {0, -1}};
    private int row;
    private int col;
    public int numIslands(char[][] grid) {
		row = grid.length;
        if(row == 0)	return 0;
        col = grid[0].length;
        this.grid = grid;
        int res = 0;
        visited = new boolean[row][col];
        for(int i = 0;i < row;i++){
            for(int j = 0;j < col;j++){
               	if(grid[i][j] == '1' && !visited[i][j]){
                    dfs(i, j);
                    res ++;
                }
            }
        }
       	return res;
    }
    
    public void dfs(int i, int j){
        visited[i][j] = true;
        //看四个方向上的
        for(int k = 0;k < 4;k++){
            int newX = i + directions[k][0];
            int newY = j + directions[k][1];
            if(isInArea(newX, newY) && grid[i][j] == '1' && !visited[newX][newY])
                dfs(newX, newY);
        }
    }
    
    public boolean isInArea(int i, int j){
        if((i >= 0 && i < row) && (j >= 0 && j < col))
            return true;
        return false;
    }
    
}
```

思路2：广度优先遍历

```java
class Solution {
    public int numIslands(char[][] grid) {

    }
}
```

思路3：并查集

```java
class Solution {
    public int numIslands(char[][] grid) {

    }
}
```

#### 130 被围绕的区域

```
给定一个二维的矩阵，包含 'X' 和 'O'（字母 O）。

找到所有被 'X' 围绕的区域，并将这些区域里所有的 'O' 用 'X' 填充。

示例:

X X X X
X O O X
X X O X
X O X X
运行你的函数后，矩阵变为：

X X X X
X X X X
X X X X
X O X X
解释:

被围绕的区间不会存在于边界上，换句话说，任何边界上的 'O' 都不会被填充为 'X'。 任何不在边界上，或不与边界上的 'O' 相连的 'O' 最终都会被填充为 'X'。如果两个元素在水平或垂直方向相邻，则称它们是“相连”的。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/surrounded-regions
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
```

要点：**任何边界上的 'O' 都不会被填充为 'X'。**

​			**任何不在边界上，或不与边界上的 'O' 相连的 'O' 最终都会被填充为 'X'。**

​			==题目不给提示，还真没想到==

思路：自己写的深度优先遍历，把在bound上面的所有O和边界O能到达的O都标记一下，剩下的可以更改为'X'

```java
class Solution {
    private char[][] board;
    private boolean[][] visited;
    private int[][] directions = {{-1, 0}, {0, 1}, {0, -1}, {1, 0}};
    private int row;
    private int col;
    public void solve(char[][] board) {
		row = board.length;
        if(row == 0)    return;
        col = board[0].length;
        visited = new boolean [row][col];
        this.board = board;
  		for(int i = 0;i < row;i++){
            for(int j = 0;j < col;j++){
                if(board[i][j] == 'O' && isBound(i, j) && !visited[i][j])
                    dfs(i, j);
            }
        }
        
        for(int i = 0;i < row;i++){
            for(int j = 0;j < col;j++){
                if(board[i][j] == 'O' && !visited[i][j])
                    board[i][j] = 'X';
            }
        }
        return;
    }
    public void dfs(int i, int j){
        visited[i][j] = true;
        for(int k = 0;k < 4;k++){
            int newX = i + directions[k][0];
            int newY = j + directions[k][1];
            if(isInArea(newX, newY) && !visited[newX][newY]  && board[newX][newY] == 'O')
                dfs(newX, newY);
        }
    }
    
    public boolean isBound(int i, int j){
        if(i == 0 || i == row - 1 || j == 0 || j == col - 1)
            return true;
        return false;
    }
      
    public boolean isInArea(int x, int y){
        return (x >= 0 && x < row && y >= 0 && y < col);
    }
}
```

#### 79 单词搜索

```
给定一个二维网格和一个单词，找出该单词是否存在于网格中。

单词必须按照字母顺序，通过相邻的单元格内的字母构成，其中“相邻”单元格是那些水平相邻或垂直相邻的单元格。同一个单元格内的字母不允许被重复使用。

示例:

board =
[
  ['A','B','C','E'],
  ['S','F','C','S'],
  ['A','D','E','E']
]

给定 word = "ABCCED", 返回 true
给定 word = "SEE", 返回 true
给定 word = "ABCB", 返回 false 

提示：

board 和 word 中只包含大写和小写英文字母。
1 <= board.length <= 200
1 <= board[i].length <= 200
1 <= word.length <= 10^3

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/word-search
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
```

思路：==自己写的超时了，不能算AC，看了题解，改了一下==

原因在于我在递归的时候保存了一个res 。然后 res |= dfs(newX, newY, depth + 1)算出来好多没有用的值，其实只要有一个正确结果就可以剪枝了，直接return就好了。==值得高兴的是，我们整体的思路完全一样==

```java
class Solution {
    private int row;
    private int col;
    private char[][] board;
    private boolean[][] visited;
    private int[][] directions = {{-1, 0}, {1, 0}, {0, 1}, {0, -1}};
    private char chs[];
    public boolean exist(char[][] board, String word) {
        row = board.length;
        col = board[0].length;
        this.board = board;
        visited = new boolean[row][col];
        chs = word.toCharArray();
        boolean resu = false;
      	for(int i = 0;i < row;i++){
            for(int j = 0;j < col;j++){
                if(dfs(i, j ,0))
                   	return true;
            }
        }
        return false;
    }
    
    public boolean dfs(int i, int j, int depth){
        if(depth == chs.length - 1){
        	return chs[depth] == board[i][j];
        }
        if(chs[depth] == board[i][j]){
            visited[i][j] = true;
        	for(int k = 0;k < 4;k++){
            	int newX = i + directions[k][0];
            	int newY = j + directions[k][1];
            	if(isInArea(newX, newY) && !visited[newX][newY]){
                    if(dfs(newX, newY, depth + 1))
        	            return true;
                }	
            }
            visited[i][j] = false;
        }
        return false;
    }
    
    public boolean isInArea(int i, int j){
        return (i >= 0 && i < row && j >= 0 && j < col);
    }
}
```

#### 17 电话号码的字母组合

```
给定一个仅包含数字 2-9 的字符串，返回所有它能表示的字母组合。

给出数字到字母的映射如下（与电话按键相同）。注意 1 不对应任何字母。

示例:

输入："23"
输出：["ad", "ae", "af", "bd", "be", "bf", "cd", "ce", "cf"].
说明:
尽管上面的答案是按字典序排列的，但是你可以任意选择答案输出的顺序

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/letter-combinations-of-a-phone-number
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
```

思路：==需要注意的一点是，字符串的问题的特殊之处在于，字符串的拼接生成新对象，因此在这一类问题上没有显式「回溯」的过程，但是如果使用 `StringBuilder` 拼接字符串就另当别论，比如如果这个题用String直接写，那就是不用清除历史状态==

看清楚题解中数组的设置方式

```java
class Solution {
    private String[] direc = {"abc","def","ghi","jkl","mno","pqrs","tuv","wxyz"};
    private StringBuffer sb = new StringBuffer();
    private List<String> resList = new ArrayList<>();

    public List<String> letterCombinations(String digits) {
        if(digits == null || digits.length() == 0)
            return resList;
        dfs(digits, 0);
        return resList;
    }

    public void dfs(String digits, int curIndex){
        if(digits.length() == sb.length()){
            resList.add(sb.toString());
            return ;
        }
        String s = direc[digits.charAt(curIndex) - '2'];
        for(char c : s.toCharArray()){
            sb.append(c);
            dfs(digits, curIndex + 1);
            sb.deleteCharAt(sb.length() - 1);
        }
        return ;
    }
}
```

#### 22 括号生成

```
数字 n 代表生成括号的对数，请你设计一个函数，用于能够生成所有可能的并且 有效的 括号组合。

示例：

输入：n = 3
输出：[
       "((()))",
       "(()())",
       "(())()",
       "()(())",
       "()()()"
     ]

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/generate-parentheses
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
```

思路：写起来不难，但是比较难想这个思路，最关键的地方如图，也是我们的剪枝策略的出处

​																				==非常漂亮的递归==

我们这个题就尝试写出用String的，可以看到省去了回溯的步骤，相当于直接dfs

![image-20201214092223246](https://gitee.com/f0rest9999/images/raw/master/20201214092230.png)

```java
class Solution {
    private List<String> resList = new ArrayList<>();
    public List<String> generateParenthesis(int n) {
        if(n == 0)	return resList;
        dfs("", n, n);
        return resList;
    }
    public void dfs(String curString, int left, int right){
        if(left == 0 && right == 0){
            resList.add(curString);
            return;
        }
        //如果左边的括号数比右边的括号数少，那么就应该剪枝
        //妙啊，我是没想到的
        if(left > right)
            return;
        if(left > 0)
            dfs(curString + "(", left - 1, right);
        if(right > 0)
            dfs(curString + ")", left, right - 1);
    }
}
```

#### 51 N皇后

```
n 皇后问题研究的是如何将 n 个皇后放置在 n×n 的棋盘上，并且使皇后彼此之间不能相互攻击。

给定一个整数 n，返回所有不同的 n 皇后问题的解决方案。

每一种解法包含一个明确的 n 皇后问题的棋子放置方案，该方案中 'Q' 和 '.' 分别代表了皇后和空位。

示例：

输入：4
输出：[
 [".Q..",  // 解法 1
  "...Q",
  "Q...",
  "..Q."],

 ["..Q.",  // 解法 2
  "Q...",
  "...Q",
  ".Q.."]
]
解释: 4 皇后问题存在两个不同的解法。
 

提示：

皇后彼此不能相互攻击，也就是说：任何两个皇后都不能处于同一条横行、纵行或斜线上。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/n-queens
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
```

思路：经典的回溯问题，我们剪枝的参考是什么呢：1.不在同一行 2.不在同一列 3. 不在同一个主对角线上 4.不在同一个副对角线上

- 对于行我们通过dfs的depth就可以限制，即每次进入一层dfs，就相当于是走到了下一行

- 对于列我们用一个数组col[]，来记录，同理，主对角线用一个main[]，副对角线用一个sub[]

  ==注意col的大小就是n，main大小应该是2 * n - 1（因为还包含负数），同理 sub大小也是2 * n - 1==

  - 对于4，main的真实范围是-3 - 3，那么我们对应到数组中的表示应该是n + depth - j - 1


  - sub的真实范围是0 - 6，对应到数组中的表示就是 j

  ![图画题解-8](https://gitee.com/f0rest9999/images/raw/master/20201214143054.jpg)

- ==还有一个小技巧，我们用一个Deque即path表示目前为止的排列，再加入resList时还原棋盘就行==

  ![image-20201214131041092](https://gitee.com/f0rest9999/images/raw/master/20201214131041.png)![image-20201214131127692](https://gitee.com/f0rest9999/images/raw/master/20201214131127.png)

  分别对应的是![image-20201214131104987](https://gitee.com/f0rest9999/images/raw/master/20201214131105.png)第一个表示【（第一行）第二列】【（第二行）第四列】【（第三行）第一列】【（第四行）第三列】

  

- **注意StringBuffer的`repeat`以及`replace`的用法**

```java
class Solution {
    private List<List<String>> resList = new ArrayList<>();
    private boolean col[];
    private boolean main[];
    private boolean sub[];
    public List<List<String>> solveNQueens(int n) {
		if(n == 0)	return resList;
        Deque<Integer> path = new ArrayDeque<>();
        col = new boolean[n];
        main = new boolean[2 * n - 1];
    	sub = new boolean[2 * n - 1];
        dfs(path, 0, n);
        return resList;
    }
    
    public void dfs(Deque<Integer> path, int depth, int n){
        if(depth == n){
        	List<String> oneBoard = convertToBoard(path, n);
            resList.add(oneBoard);
            return;
        }
        //针对下标为depth的行的每一  列   进行遍历是否有可以放置的
        for(int j = 0;j < n;j++){
            if(!col[j] && !main[n + depth - j - 1] && !sub[j + depth]){
                col[j] = !col[j];
                main[n + depth - j - 1] = !main[n + depth - j - 1];
               	sub[j + depth] = !sub[j + depth];
                
                path.add(j);
                dfs(path, depth + 1, n);
                path.removeLast();
                
                col[j] = !col[j];
                main[n + depth - j - 1] = !main[n + depth - j - 1];
               	sub[j + depth] = !sub[j + depth];
            }
        }
    }
    public List<String> convertToBoard(Deque<Integer> path, int n){
        List<String> res = new ArrayList<>();
        for(Integer num : path){
            StringBuffer sb = new StringBuffer();
            sb.append(".".repeat(Math.max(0, n)));
            sb.replace(num, num + 1, "Q");
            res.add(sb.toString());
        }
        return res;
    }
}
```

#### 37 解数独

```
编写一个程序，通过填充空格来解决数独问题。

一个数独的解法需遵循如下规则：

数字 1-9 在每一行只能出现一次。
数字 1-9 在每一列只能出现一次。
数字 1-9 在每一个以粗实线分隔的 3x3 宫内只能出现一次。
空白格用 '.' 表示。
```

​														![image-20201214132429493](https://gitee.com/f0rest9999/images/raw/master/20201214132429.png)					![image-20201214132506831](https://gitee.com/f0rest9999/images/raw/master/20201214132506.png)

```
一个数独。

答案被标成红色。

提示：

给定的数独序列只包含数字 1-9 和字符 '.' 。
你可以假设给定的数独只有唯一解。
给定数独永远是 9x9 形式的。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/sudoku-solver
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
```

思路：row代表第 i 行第几个数有没有出现过，col代表第 j 列第几个数有没有出现过，==主要是没想出来`在每一个以粗实线分隔的 3x3 宫内只能出现一次`该怎么表示，画图找规律，i / 3  * 3 + j / 3就可以索引到那个索引块了==

![图画题解-9](https://gitee.com/f0rest9999/images/raw/master/20201214143116.jpg)

```java
class Solution {
    private boolean row[][] = new boolean [9][9];
    private boolean col[][] = new boolean [9][9];
    private boolean block[][] = new boolean [9][9];
    public void solveSudoku(char[][] board) {
		//初始化上面三个数组
        for(int i = 0;i < 9;i++){
            for(int j = 0;j < 9;j++){
            	if(board[i][j] != '.'){
                    int num = board[i][j] - '1';
                    row[i][num] = true;
                    col[j][num] = true;
                    block[i / 3 * 3 + j / 3][num] = true;
                }
            }
        }
        dfs(board, 0, 0);
        return;
    }
    //i, j 表示从哪个坐标开始
    public boolean dfs(char[][] board, int i, int j){
        //按行再按列找到第一个可以更改的数
        while(board[i][j] != '.'){
            if(++j >= 9){
                j = 0;
                i++;
            }
            if(i >= 9)
                return true;
        }
        for(int k = 0;k < 9;k++){
            int blockIndex = i / 3 * 3 + j / 3;
            if(!row[i][k] && !col[j][k] && !block[blockIndex][k]){
                board[i][j] = (char)(k + '1');
                row[i][k] = !row[i][k];
                col[j][k] = !col[j][k];
                block[blockIndex][k] = !block[blockIndex][k];
                if(dfs(board, i, j))
                    return true;
                else{
                    row[i][k] = !row[i][k];
                	col[j][k] = !col[j][k];
               	 	block[blockIndex][k] = !block[blockIndex][k];
                    board[i][j] = '.';
                }
            }
        }
        return false;
    }
}
```

#### 40 组合总和2

```
给定一个数组 candidates 和一个目标数 target ，找出 candidates 中所有可以使数字和为 target 的组合。

candidates 中的每个数字在每个组合中只能使用一次。

说明：

所有数字（包括目标数）都是正整数。
解集不能包含重复的组合。 
示例 1:

输入: candidates = [10,1,2,7,6,1,5], target = 8,
所求解集为:
[
  [1, 7],
  [1, 2, 5],
  [2, 6],
  [1, 1, 6]
]
示例 2:

输入: candidates = [2,5,2,1,2], target = 5,
所求解集为:
[
  [1,2,2],
  [5]
]

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/combination-sum-ii
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
```

思路：用used和begin 进行剪枝

```java
class Solution {
    private int target;
    private List<List<Integer>> resList = new ArrayList<>();
    private boolean used[];
    private int len;
    public List<List<Integer>> combinationSum2(int[] candidates, int target) {
        this.len = candidates.length;
        if(len == 0)    return resList;
        this.target = target;
        used = new boolean[len];
        Arrays.sort(candidates);
        Deque<Integer> path = new ArrayDeque<>();
        dfs(candidates, 0, path, 0);
        return resList;
    }
    public void dfs(int[] candidates, int sum, Deque<Integer> path, int begin){
        if(sum > target)
            return;
        if(sum == target){
            resList.add(new ArrayList<>(path));
            return;
        }
        for(int i = begin;i < len;i++){
            if(i > 0 && candidates[i - 1] == candidates[i] && !used[i - 1])
                continue;
            
            if(!used[i]){
                used[i] = true;
                sum += candidates[i];
                path.add(candidates[i]);

                dfs(candidates, sum, path, i);

                sum -= candidates[i];
                path.removeLast();
                used[i] = false;
            }
        }
    }
}
```

#### 16 最接近的三数之和

```
给定一个包括 n 个整数的数组 nums 和 一个目标值 target。找出 nums 中的三个整数，使得它们的和与 target 最接近。返回这三个数的和。假定每组输入只存在唯一答案。

示例：

输入：nums = [-1,2,1,-4], target = 1
输出：2
解释：与 target 最接近的和是 2 (-1 + 2 + 1 = 2) 。
提示：

3 <= nums.length <= 10^3
-10^3 <= nums[i] <= 10^3
-10^4 <= target <= 10^4

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/3sum-closest
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
```

思路：可以有别的办法，但是我们这儿直接采用了暴力回溯，加上一定的剪枝，也通过了哈哈

​			就是比较慢

- 比较快的一个思路是，先排序数组，然后外层循环定a的位置，然后内层循环用双指针，来确定b和c的位置，保存值，然后遍历下一个a

```java
class Solution {
    //我就用回溯试试不亏吧
    private int minDiff = Integer.MAX_VALUE;
    private boolean used[];
    private int k = 3;
    private int target;
    private boolean symbol = true;
    public int threeSumClosest(int[] nums, int target) {
        int length = nums.length;
        if(length == 0) return 0;
        this.target = target;
        used = new boolean[length];
        Arrays.sort(nums);
        dfs(nums, 0, 0);
        if(symbol)
            return minDiff + target;
        return -minDiff + target;
    }
    public void dfs(int[] nums, int sum, int depth){
        if(depth == k){
            int diff = sum - target;
            if(Math.abs(diff) < minDiff){
                symbol = diff >= 0;
                minDiff = Math.abs(diff);
            }
            return;
        }
        for(int i = 0;i < nums.length;i++){
            if(i > 0 && !used[i - 1] && nums[i] == nums[i - 1])
                continue;
            if(!used[i]){
                used[i] = true;
                sum += nums[i];

                dfs(nums, sum, depth + 1);

                used[i] = false;
                sum -= nums[i];
            }        
        }
        return;
    }
}
```



## 数组

####204 计算质数

```markdown
统计所有小于非负整数 n 的质数的数量。
示例 1：

输入：n = 10
输出：4
解释：小于 10 的质数一共有 4 个, 它们是 2, 3, 5, 7 。
示例 2：

输入：n = 0
输出：0
示例 3：

输入：n = 1
输出：0
提示：

0 <= n <= 5 * 106
```

思路一：普通枚举

```java
class Solution {
    public int countPrimes(int n) {
		int res = 0;
        for(int i = 2;i < n;i++){
            res += isPrime(i) ? 1 : 0;
        }
        return res;
    }
    public boolean isPrime(int x){
        for(int i = 2;i * i <= x;i++){
            if(x % i == 0)
                return false;
        }
        return true;
    }
}
```

思路二：经典的厄拉多塞筛法

```java
class Solution {
    public int countPrimes(int n) {
		int count = 0;
        boolean [] isPrime = new boolean[n];
        //假设它们都是质数先
        Arrays.fill(isPrime, true);
        if(n > 2)	count ++;
        //从3开始计算,且只计算奇数，偶数不用考虑的
        for(int i = 3;i < n;i += 2){
            if(isPrime[i]){
                //把这些质数的奇数倍都设置成非质数,同样的，偶数倍不用考虑
                for(int j = 3;j * i < n ;j += 2){
                    isPrime[i * j] = false;
                }
                count ++;
            }
        }
   		return count;
    }
}
```

#### 56 合并区间

```
给出一个区间的集合，请合并所有重叠的区间。

示例 1:

输入: intervals = [[1,3],[2,6],[8,10],[15,18]]
输出: [[1,6],[8,10],[15,18]]
解释: 区间 [1,3] 和 [2,6] 重叠, 将它们合并为 [1,6].
示例 2:

输入: intervals = [[1,4],[4,5]]
输出: [[1,5]]
解释: 区间 [1,4] 和 [4,5] 可被视为重叠区间。
注意：输入类型已于2019年4月15日更改。 请重置默认代码定义以获取新方法签名。

提示：

intervals[i][0] <= intervals[i][1]

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/merge-intervals
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
```

思路：先按起始进行排序，然后依次判断尾端的情况，==排序API的使用以及List转化为数组的方法==

```java
class Solution {
    public int[][] merge(int[][] intervals) {
        if(intervals.length == 0)	return new int[0][2];
    	Arrays.sort(intervals, new Comparator<int[]>() {
            public int compare(int[] intervals1, int[] intervals2){
                return intervals1[0] - intervals2[0];
            }
        } );
        List<int[]> resList = new LinkedList<>();
        for(int i = 0;i < intervals.length;i++){
            int left = intervals[i][0];
            int right = intervals[i][1];
            if(resList.isEmpty() || resList.get(resList.size() - 1)[1] < left)
                resList.add(intervals[i]);
            else
               resList.get(resList.size() - 1)[1] = Math.max(resList.get(resList.size() - 1)[1], right);
        }
        return resList.toArray(new int[resList.size()][]);
    }
}
```

#### 59 螺旋矩阵2

```
给定一个正整数 n，生成一个包含 1 到 n2 所有元素，且元素按顺时针顺序螺旋排列的正方形矩阵。

示例:

输入: 3
输出:
[
 [ 1, 2, 3 ],
 [ 8, 9, 4 ],
 [ 7, 6, 5 ]
]

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/spiral-matrix-ii
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
```

思路：正常做好吧，注意边界处理

```java
class Solution {
    private int[][] directions = new int[][]{{0, 1}, {1,  0}, {0, -1}, {-1, 0}}; 
    private boolean[][] visited; 
    private int row;
    private int col;
    private int count = 1;
    public int[][] generateMatrix(int n) {
        this.row = n;
        this.col = n;
        int[][] res = new int[n][n];
        visited = new boolean[n][n];
        make(res, 0, 0);
    	return res;
    }
    public void make(int[][] res, int i, int j){
        int dire = 0;
        while(count <= row * col){
        	res[i][j] = count ++;
            visited[i][j] = true;
            if(count > row * col)	break;
            int newX = i + directions[dire][0];
            int newY = j + directions[dire][1];
            while(!(isInArea(newX, newY) && !visited[newX][newY])){
                dire = (dire + 1) % 4;
                newX = i + directions[dire][0];
                newY = j + directions[dire][1];
            }
            i = newX;
            j = newY;
        }
    }
    public boolean isInArea(int i, int j){
    	return (i >= 0 && i < row && j >= 0 &&j < col);
    }
}
```

#### 48 旋转图像

```
给定一个 n × n 的二维矩阵表示一个图像。

将图像顺时针旋转 90 度。

说明：

你必须在原地旋转图像，这意味着你需要直接修改输入的二维矩阵。请不要使用另一个矩阵来旋转图像。

示例 1:

给定 matrix = 
[
  [1,2,3],
  [4,5,6],
  [7,8,9]
],

原地旋转输入矩阵，使其变为:
[
  [7,4,1],
  [8,5,2],
  [9,6,3]
]
示例 2:

给定 matrix =
[
  [ 5, 1, 9,11],
  [ 2, 4, 8,10],
  [13, 3, 6, 7],
  [15,14,12,16]
], 

原地旋转输入矩阵，使其变为:
[
  [15,13, 2, 5],
  [14, 3, 4, 1],
  [12, 6, 8, 9],
  [16, 7,10,11]
]

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/rotate-image
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
```

思路：==**先转置，然后再镜像对称，太方便了！**==

```java
class Solution {
    public void rotate(int[][] matrix) {
		int row = matrix.length;
        int col = matrix[0].length;
        //转置
        for(int i = 0;i < row;i++){
            for(int j = i;j < col;j++){
                int temp = matrix[i][j];
                matrix[i][j] = matrix[j][i];
                matrix[j][i] = temp;
            }
        }
        //镜像对称,同一行的沿中轴线对称
		for(int i = 0;i < row;i++){
            for(int j = 0;j < col / 2;j++){
                int temp = matrix[i][j];
                matrix[i][j] = matrix[i][col - j - 1];
                matrix[i][col - j - 1] = temp;
            }
        }        
    }
}
```

#### 73 矩阵置零

```
给定一个 m x n 的矩阵，如果一个元素为 0，则将其所在行和列的所有元素都设为 0。请使用原地算法。

示例 1:

输入: 
[
  [1,1,1],
  [1,0,1],
  [1,1,1]
]
输出: 
[
  [1,0,1],
  [0,0,0],
  [1,0,1]
]
示例 2:

输入: 
[
  [0,1,2,0],
  [3,4,5,2],
  [1,3,1,5]
]
输出: 
[
  [0,0,0,0],
  [0,4,5,0],
  [0,3,1,0]
]
进阶:

一个直接的解决方案是使用  O(mn) 的额外空间，但这并不是一个好的解决方案。
一个简单的改进方案是使用 O(m + n) 的额外空间，但这仍然不是最好的解决方案。
你能想出一个常数空间的解决方案吗？

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/set-matrix-zeroes
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
```

思路1：相当于一个简单的floodfill，为了练习这个先写了一个dfs递归,但是也要先用一个source矩阵标识这个矩阵是否是原始0

```java
class Solution {
    private int[][] directions = new int[][]{{-1, 0}, {1, 0}, {0, 1}, {0, -1}};
    private int row;
    private int col;
    private int[][] matrix;
    private boolean[][] source;
    public void setZeroes(int[][] matrix) {
        this.row = matrix.length;
        if(row == 0) return;
        this.col = matrix[0].length;
        this.matrix = matrix;
        source = new boolean[row][col];
        for(int i = 0;i < row;i++){
            for(int j = 0;j < col;j++){
                if(matrix[i][j] == 0)
                    source[i][j] = true;
            }
        }
        for(int i = 0;i < row;i++){
            for(int j = 0;j < col;j++){
                if(source[i][j] && matrix[i][j] == 0)
                    extend(i, j, true, 0);
            }
        }
        return;
    }

    public void extend(int i, int j, boolean isSource, int dire){
        matrix[i][j] = 0;
        if(isSource){
            for(int k = 0;k < 4;k++){
                int newX = i + directions[k][0];
                int newY = j + directions[k][1];
                if(inArea(newX, newY))
                    extend(newX, newY, false, k);
            }
        }
        else{
            int newX = i + directions[dire][0];
            int newY = j + directions[dire][1];
            if(inArea(newX, newY))
                extend(newX, newY, false, dire);
        }
    }

    public boolean inArea(int i, int j){
        return (i >= 0 && i < row && j >= 0 && j < col);
    }
}
```

思路2：用第一行和第一列分别标记对应列或者对应行有没有0，即有原始0就更新对应得第一行上和第一列的值，但是要注意第一行和第一列本身就有0的情况，详见注释

```java
class Solution {
    public void setZeroes(int[][] matrix) {
        int row = matrix.length;
        if(row == 0)    return;
        int col = matrix[0].length;
        boolean has0InfirstCol = false;
        boolean has0InfirstRow = false;
        //先遍历第一行
        for(int j = 0;j < col;j++){
            if(matrix[0][j] == 0){
                has0InfirstRow = true;
                break;
            }
        }
        //遍历第一列
        for(int i = 0;i < row;i++){
            if(matrix[i][0] == 0){
                has0InfirstCol = true;
                break;
            }
        }
        //把矩阵第一行和第一列作为索引
        //注意要从（1，1）开始遍历
        for(int i = 1;i < row;i++){
            for(int j = 1;j < col;j++){
                if(matrix[i][j] == 0){
                    matrix[i][0] = 0;
                    matrix[0][j] = 0;
                }
            }
        }
        //遍历第一行并且更改值
        for(int j = 1;j < col;j++){
            if(matrix[0][j] == 0){
                for(int i = 0;i < row;i++)
                    matrix[i][j] = 0;
            }
        }
        //遍历第一列并且更改值
        for(int i = 1;i < row;i++){
            if(matrix[i][0] == 0){
                for(int j = 0;j < col;j++)
                    matrix[i][j] = 0;
            }
        }
        //处理第一行和第一列的特殊情况
        if(has0InfirstRow){
            for(int j = 0;j < col;j++)
                matrix[0][j] = 0;
        }
        if(has0InfirstCol){
            for(int i = 0;i < row;i++)
                matrix[i][0] = 0;
        }
    }
}
```

#### 1664 生成平衡数组的方案数

```
给你一个整数数组 nums 。你需要选择 恰好 一个下标（下标从 0 开始）并删除对应的元素。请注意剩下元素的下标可能会因为删除操作而发生改变。

比方说，如果 nums = [6,1,7,4,1] ，那么：

选择删除下标 1 ，剩下的数组为 nums = [6,7,4,1] 。
选择删除下标 2 ，剩下的数组为 nums = [6,1,4,1] 。
选择删除下标 4 ，剩下的数组为 nums = [6,1,7,4] 。
如果一个数组满足奇数下标元素的和与偶数下标元素的和相等，该数组就是一个 平衡数组 。

请你返回删除操作后，剩下的数组 nums 是 平衡数组 的 方案数 。

示例 1：
输入：nums = [2,1,6,4]
输出：1
解释：
删除下标 0 ：[1,6,4] -> 偶数元素下标为：1 + 4 = 5 。奇数元素下标为：6 。不平衡。
删除下标 1 ：[2,6,4] -> 偶数元素下标为：2 + 4 = 6 。奇数元素下标为：6 。平衡。
删除下标 2 ：[2,1,4] -> 偶数元素下标为：2 + 4 = 6 。奇数元素下标为：1 。不平衡。
删除下标 3 ：[2,1,6] -> 偶数元素下标为：2 + 6 = 8 。奇数元素下标为：1 。不平衡。
只有一种让剩余数组成为平衡数组的方案。
示例 2：

输入：nums = [1,1,1]
输出：3
解释：你可以删除任意元素，剩余数组都是平衡数组。
示例 3：

输入：nums = [1,2,3]
输出：0
解释：不管删除哪个元素，剩下数组都不是平衡数组。
 
提示：
1 <= nums.length <= 105
1 <= nums[i] <= 104

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/ways-to-make-a-fair-array
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
```

思路：对于每一个删除方案，我们只要计算左边的所有偶数位加上右边的所有偶数位 是否 等于 左边所有奇数位加上右边所有奇数位

所以我们先用两个数组存储所有偶数位和奇数位的和（小DP）,每删除一个元素，它右边的所有奇数位变成偶数位，偶数位变奇数位

所以我们必须交换一下这两个的值

```java
class Solution {
    public int waysToMakeFair(int[] nums) {
        int len = nums.length;
        //到目前为止奇数下标位置的和
    	int[] oddSum = new int[len];    
        //到目前为止偶数下标位置的和
    	int[] evenSum = new int[len];
        evenSum[0] = nums[0];
    	for(int i = 1;i < len;i++){
            if(i % 2 == 0){
                evenSum[i] = evenSum[i - 1] + nums[i];
                oddSum[i] = oddSum[i - 1];
            }else{
                oddSum[i] = oddSum[i - 1] + nums[i];
                evenSum[i] = evenSum[i - 1];
            }
        }
    	int res = 0;
        for(int i = 0;i < len;i++){
            int left_odd = i >= 1 ? oddSum[i - 1] : 0;
            int left_even = i >= 1 ? evenSum[i - 1] : 0;
            int right_odd = oddSum[len - 1] - oddSum[i];
            int right_even = evenSum[len - 1] - evenSum[i];
            
            int temp = right_odd;
            right_odd = right_even;
            right_even = temp;
            
            if(right_odd + left_odd == right_even + left_even)
                res ++;
        }
        return res;
    }
}
```





## SQL

#### 184 部门工资最高的员工

```
Employee 表包含所有员工信息，每个员工有其对应的 Id, salary 和 department Id。

+----+-------+--------+--------------+
| Id | Name  | Salary | DepartmentId |
+----+-------+--------+--------------+
| 1  | Joe   | 70000  | 1            |
| 2  | Jim   | 90000  | 1            |
| 3  | Henry | 80000  | 2            |
| 4  | Sam   | 60000  | 2            |
| 5  | Max   | 90000  | 1            |
+----+-------+--------+--------------+
Department 表包含公司所有部门的信息。

+----+----------+
| Id | Name     |
+----+----------+
| 1  | IT       |
| 2  | Sales    |
+----+----------+
编写一个 SQL 查询，找出每个部门工资最高的员工。对于上述表，您的 SQL 查询应返回以下行（行的顺序无关紧要）。

+------------+----------+--------+
| Department | Employee | Salary |
+------------+----------+--------+
| IT         | Max      | 90000  |
| IT         | Jim      | 90000  |
| Sales      | Henry    | 80000  |
+------------+----------+--------+
解释：

Max 和 Jim 在 IT 部门的工资都是最高的，Henry 在销售部的工资最高。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/department-highest-salary
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
```

思路：==见过这么用IN的吗==

```sql
Select Department.Name As Department, Employee.Name As Employee, Salary From Department, Employee
	Where Department.Id = Employee.DepartmentId 
		and (Employee.DepartmentId, Salary) In (
        	Select DepartmentId, MAX(Salary)
           	From Employee
            Group by DepartmentId
        )
```



