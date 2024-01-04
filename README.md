
# 07Backtracking

第77题. 组合
力扣题目链接(opens new window)

给定两个整数 n 和 k，返回 1 ... n 中所有可能的 k 个数的组合。

示例: 输入: n = 4, k = 2 输出: [ [2,4], [3,4], [2,3], [1,2], [1,3], [1,4], ]

解题说明：需要暴力才能完成的组合问题，可以考虑用回溯法。回溯写法，一般是在globle简历全局参数，一个output，另一个是output的path，然后再题目函数中call 一个helper函数，传入参数：给定数组，startIndex，
回溯三要素：
1. 返回值和参数： void, 参数是传入的数组跟 一个记录每次循环的startIndex, 如果不能重复取之前取过的树，则startIndex在递归的时候== i + 1;
2. 递归终止条件：当path.size == k, 可以收集结果。
3. 递归单层逻辑，for循环遍历数组，每得到一个值，就先存入path，然后往下递归，递归传入原来的数组跟startIndex= i+1，然后回溯：把path的最后一个存进去的值remove，因为要回上层，继续往下递归了。
4. 剪枝优化：如果发现剩下的数已经不够形成path的size，就可以不用继续遍历，所以在循环中可以写：i <= n - (k - path.size()) + 1;
   
代码实现：
class Solution {
    List<List<Integer>> result= new ArrayList<>();
    LinkedList<Integer> path = new LinkedList<>();
    public List<List<Integer>> combine(int n, int k) {
        backtracking(n,k,1);
        return result;
    }

    public void backtracking(int n,int k,int startIndex){
        if (path.size() == k){
            result.add(new ArrayList<>(path));
            return;
        }
        for (int i =startIndex;i<=n;i++){
            path.add(i);
            backtracking(n,k,i+1);
            path.removeLast();
        }
    }
}

216.组合总和III
力扣题目链接(opens new window)

找出所有相加之和为 n 的 k 个数的组合。组合中只允许含有 1 - 9 的正整数，并且每种组合中不存在重复的数字。

说明：

所有数字都是正整数。
解集不能包含重复的组合。
示例 1: 输入: k = 3, n = 7 输出: [[1,2,4]]

示例 2: 输入: k = 3, n = 9 输出: [[1,2,6], [1,3,5], [2,3,4]]

解题思路：本题属于对输出有要求，因此需要在遍历时加入限制条件，sum。
1. 返回值和参数：nums, k , n, sum
2. 递归终止条件：当path.size == k && sum == 0； 收获结果.
3. 单层递归，for循环，将每个值加入到path，然后sum - 这个值，就变小。并且回溯时删除最后一个值，也把sum加回去。
4. 剪枝： 如果剩下的数已经不够path.size了，就可以不遍历，还有如果sum已经减太多了，编程负数，不可能等于0，也可以直接跳过。

代码实现：
class Solution {
	List<List<Integer>> result = new ArrayList<>();
	LinkedList<Integer> path = new LinkedList<>();

	public List<List<Integer>> combinationSum3(int k, int n) {
		backTracking(n, k, 1, 0);
		return result;
	}

	private void backTracking(int targetSum, int k, int startIndex, int sum) {
		// 减枝
		if (sum > targetSum) {
			return;
		}

		if (path.size() == k) {
			if (sum == targetSum) result.add(new ArrayList<>(path));
			return;
		}

		// 减枝 9 - (k - path.size()) + 1
		for (int i = startIndex; i <= 9 - (k - path.size()) + 1; i++) {
			path.add(i);
			sum += i;
			backTracking(targetSum, k, i + 1, sum);
			//回溯
			path.removeLast();
			//回溯
			sum -= i;
		}
	}
}    

