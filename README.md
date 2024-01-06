
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
1. 返回值和参数：  n, k, startIndex, sum
2. 递归终止条件：当path.size == k && sum > targetSum； 收获结果.
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

39. 组合总和

给定一个无重复元素的数组 candidates 和一个目标数 target ，找出 candidates 中所有可以使数字和为 target 的组合。

candidates 中的数字可以无限制重复被选取。

说明：

所有数字（包括 target）都是正整数。
解集不能包含重复的组合。
示例 1：

输入：candidates = [2,3,6,7], target = 7,
所求解集为： [ [7], [2,2,3] ]
示例 2：

输入：candidates = [2,3,5], target = 8,
所求解集为： [ [2,2,2,2], [2,3,3], [3,5] ]

解题思路： 查询的的数组中的值是可以重复取的，所以每次都可以从当前位开始取，startIndex = i ，直到和超过target。
1. 返回值和参数： void， nums, startIndex, target;
2. 递归终止条件：当target == 0时可以收获结果，当target < 0 时可以不循环直接return。
3. 单层递归逻辑：每次选取一个num，都加入到path，sum += num, 然后递归 startIndex = i;

代码实现：
class Solution {
    List<List<Integer>> output = new ArrayList<>();
    List<Integer> path = new ArrayList<>();
    public List<List<Integer>> combinationSum(int[] candidates, int target) {
        Arrays.sort(candidates);
        hepler(candidates, target, 0);
        return output;
    }
    public void hepler(int[] candidates, int target, int startIndex) {
        if(target < 0){
            return;
        }
        if(target == 0){
            output.add(new ArrayList(path));
            return;
        }
        for(int i = startIndex; i < candidates.length &&  candidates[i] <= target ; i++){
            path.add(candidates[i]);
            hepler(candidates, target - candidates[i] , i);
            path.remove(path.size() - 1);
        }
    }
}

40.组合总和II

给定一个数组 candidates 和一个目标数 target ，找出 candidates 中所有可以使数字和为 target 的组合。

candidates 中的每个数字在每个组合中只能使用一次。

说明： 所有数字（包括目标数）都是正整数。解集不能包含重复的组合。

示例 1:
输入: candidates = [10,1,2,7,6,1,5], target = 8,
所求解集为:
[[1, 7],[1, 2, 5],[2, 6],[1, 1, 6]]
示例 2:
输入: candidates = [2,5,2,1,2], target = 5,
所求解集为:
[[1,2,2],[5]]

解题思路：
对于数组中右重复的情况，要拍讯，然后考虑在for循环中如果取某个数，就要略过，所以需要一个used来保存。当nums[i] == nums[i - 1] &* used[i - 1] == false, 直接return 跳过,这是代表for循环上的去重。因为树枝上used[i - 1] == true；另外取过的数不重新取，所以startIndex= i+1；

class Solution {
    List<List<Integer>> output = new ArrayList<>();
    List<Integer> path = new ArrayList<>();
    public List<List<Integer>> combinationSum2(int[] candidates, int target) {
        boolean[] used = new boolean[candidates.length];
        Arrays.sort(candidates);
        helper(candidates, target, used, 0);
        return output;
    }
    public void helper(int[] candidates, int target, boolean[] used, int startIndex) {
        if(target == 0){
            output.add(new ArrayList<>(path));
            return;
        }
       
        for(int i = startIndex; i < candidates.length &&  target >= 0; i++){
            if(i > 0 && candidates[i] == candidates[i-1] && used[i-1] == false){
                continue;
            }
            used[i] = true;
            path.add(candidates[i]);
            helper(candidates, target-candidates[i], used, i+1);
            path.remove(path.size()-1);
            used[i] = false;
        }
    }
}

131.分割回文串
力扣题目链接(opens new window)

给定一个字符串 s，将 s 分割成一些子串，使每个子串都是回文串。

返回 s 所有可能的分割方案。

示例: 输入: "aab" 输出: [ ["aa","b"], ["a","a","b"] ]

解题思路： 采用递归的方式遍历下标，然后substring来取字符串，对每次取得字符串判断是否回文，是的话存入到output。
1. 返回值和参数： void， 原始字符串s, startIndex。
2. 终止条件，如果 startIndex 大于= s.length， 说明已经遍历到最后了。
3. 单层递归逻辑，每次都根据startIndex，跟i作为范围截取string，对string进行判断，是回文的话加入到path里，然后递归startIndex == i+1，因为取过的字符不重复取。

class Solution {
    List<List<String>> output = new ArrayList<>();
    List<String> path = new ArrayList<>();
    public List<List<String>> partition(String s) {
        helper(s, 0);
        return output;
    }
    public void helper(String s, int startIndex) {
        if(startIndex >= s.length()){
            output.add(new ArrayList<>(path));
            return;
        }
        for(int i = startIndex; i < s.length(); i++){
            if(isPalindrome(s, startIndex, i)){
                path.add(s.substring(startIndex, i+1));
            helper(s, i+1);
            path.remove(path.size()-1);
            }
     
        }
    }
    public boolean isPalindrome(String s, int start, int end){
        while(start < end){
            if(s.charAt(start) != s.charAt(end)){
                return false;
            }
            start++;
            end--;
        }
        return true;
    }
}

93.复原IP地址
力扣题目链接(opens new window)

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

解题思路：每次取到一个index后得到对应的数字，做判断，符合的话加入到path，
1. 参数： 原字符串s，startIndex= 0；
2. 终止条件，如果遍历到最后了startIndex >= s.length,并且path.size == 4， 那就可以把这个path存到output里面。然后return。如果path.size == 4，但是还没遍历到最后，那就不对，可以直接返回。
3. 单层逻辑，每次遍历，用startIndex 跟 i作为范围取substring, 然后判断这个string valid的话：就存到path里面加个点，然后递归：startIndex= i+1；path要回溯移除最后一位。

代码实现：
class Solution {
    List<String> output = new ArrayList<>();
    List<String> path = new ArrayList<>();
    public List<String> restoreIpAddresses(String s) {
        helper(s, 0);
        return output;
    }
    public void helper(String s, int startIndex){
        if(startIndex >= s.length() && path.size() == 4){
            output.add(String.join(".", path));
            return;
        }
        if (path.size() == 4) return;
        for(int i = startIndex; i < s.length(); i++){
            String str = s.substring(startIndex,i+1);
            if(isValid(str)){
                path.add(str);
                helper(s,i+1);
                path.remove(path.size() -1);
            }
        }
    }
    public boolean isValid(String s){
        if(s.length() >3) return false;
        int value = Integer.valueOf(s);  
        if (value > 0 && s.charAt(0) == '0') return false; // 023
        if (value ==0 && s.length() > 1) return false; // 00
        if(value <= 255 && value >= 0 ){
            return true;
        }
        return false;
    }
}

78.子集
力扣题目链接(opens new window)

给定一组不含重复元素的整数数组 nums，返回该数组所有可能的子集（幂集）。

说明：解集不能包含重复的子集。

示例: 输入: nums = [1,2,3] 输出: [ [3],   [1],   [2],   [1,2,3],   [1,3],   [2,3],   [1,2],   [] ]

解题思路：本身数组不重复，取所有子集的话，就是全部组合。没有去重问题。
1. 参数：输入的数组nums, startIndex。
2. 终止条件： 当startIndex >= nums.length，就遍历结束了。
3. 单层遍历逻辑，把每次遍历的下标值放到path，然后递归startIndex == i+1，remove path 的最后一个值。每一个path都存到output里面。

代码实现：
class Solution {
    List<List<Integer>> output = new ArrayList<>();
    List<Integer> path = new ArrayList<>();
    public List<List<Integer>> subsets(int[] nums) {
        helper(nums, 0);
        return output;
    }
    public void helper(int[] nums, int startIndex){
        output.add(new ArrayList<>(path));
        if(startIndex >= nums.length) return;
        for(int i = startIndex; i < nums.length; i++){
            path.add(nums[i]);
            helper(nums, i+1);
            path.remove(path.size() - 1);
        } 
    }
}

90.子集II
力扣题目链接(opens new window)

给定一个可能包含重复元素的整数数组 nums，返回该数组所有可能的子集（幂集）。

说明：解集不能包含重复的子集。

示例:

输入: [1,2,2]
输出: [ [2], [1], [1,2,2], [2,2], [1,2], [] ]

解题思路：要在原来的数组里去重，就是采用used数组来记录，当used[i-1] == used[i]&& used[i-1]= false的时候忽略continue。
1. 参数：输入的数组nums, startIndex, used。
2. 终止条件： 当startIndex >= nums.length，就遍历结束了。
3. 单层遍历逻辑，把每次遍历的下标对应的值满足条件的话忽略，其他放到path，used[i] == true, 然后递归startIndex == i+1，remove path 的最后一个值，used[i] == fasle。每一个path都存到output里面。

代码实现：
class Solution {
    List<List<Integer>> output = new ArrayList<>();
    List<Integer> path = new ArrayList<>();
    public List<List<Integer>> subsetsWithDup(int[] nums) {
        boolean[] used = new boolean[nums.length];
        Arrays.sort(nums);
        helper(nums, 0, used);
        return output;
    }
    public void helper(int[] nums, int startIndex, boolean[] used){
        output.add(new ArrayList<>(path));
        for(int i = startIndex; i < nums.length; i++){
            if(i >0 && nums[i] == nums[i - 1] && used[i-1] == false){
                continue;
            }
            used[i] = true;
            path.add(nums[i]);
            helper(nums, i+1, used);
            path.remove(path.size()-1);
            used[i] = false;
        }
    }
}

491.递增子序列
力扣题目链接(opens new window)

给定一个整型数组, 你的任务是找到所有该数组的递增子序列，递增子序列的长度至少是2。

示例:

输入: [4, 6, 7, 7]
输出: [[4, 6], [4, 7], [4, 6, 7], [4, 6, 7, 7], [6, 7], [6, 7, 7], [7,7], [4,7,7]]
说明:

给定数组的长度不会超过15。
数组中的整数范围是 [-100,100]。
给定数组中可能包含重复数字，相等的数字应该被视为递增的一种情况。

解题思路，本题不能排序，因为一定排序，就会变成一个自增序列，失去他们原来的下标顺序。所以也不能使用used数组来做标记了，这种情况也可以引入set数组来去重。通过比较nus[i] 跟path的最后一个数，判断是否满足递增。是的话计入path，
1. 参数： 传入的原始数组， startIndex。
2. 终止条件，当path的size >= 2 的时候手机结果。
3. 单层递归逻辑，每个数都存入path，然后递归，startIndex == i+1（不往回取）。

代码实现
class Solution {
    List<List<Integer>> output = new ArrayList<>();
    List<Integer> path = new ArrayList<>();
    public List<List<Integer>> findSubsequences(int[] nums) {
        helper(nums, 0);
        return output;
    }
    public void helper(int[] nums, int startIndex) {
        if(path.size() >= 2){
            output.add(new ArrayList<>(path));
        }
        if(startIndex >= nums.length){
            return;
        }
        HashSet<Integer> set = new HashSet<>();
        for(int i = startIndex; i < nums.length; i++){
            if((path.isEmpty() ||  nums[i] >= path.get(path.size()-1)) && !set.contains(nums[i])){
                set.add(nums[i]);
                path.add(nums[i]);
                helper(nums, i+1);
                path.remove(path.size()-1);
            }
        }
    }
}

46.全排列
力扣题目链接(opens new window)

给定一个 没有重复 数字的序列，返回其所有可能的全排列。

示例:

输入: [1,2,3]
输出: [ [1,2,3], [1,3,2], [2,1,3], [2,3,1], [3,1,2], [3,2,1] ]
#算法公开课
《代码随想录》算法视频公开课 (opens new window)：组合与排列的区别，回溯算法求解的时候，有何不同？| LeetCode：46.全排列 (opens new window)，相信结合视频再看本篇题解，更有助于大家对本题的理解。

解题思路：全排列是可以往回取数的，所以在递归的时候startIndex一直等于0；也就是每次都从开始循环取数，但是不要取上次取过的数，跳过call自己的那个值，可以用过used数组来跳过。
参数：只有原来的数组本身。
终止条件，当path的size == 3的时候，可以收集结果，等到整个数组遍历结束了，自然结束。

代码实现：
class Solution {
    List<List<Integer>> output = new ArrayList<>();
    List<Integer> path = new ArrayList<>();
    public List<List<Integer>> permute(int[] nums) {
        boolean[] used = new boolean[nums.length];
        helper(nums, used);
        return output;
    }
    public void helper(int[] nums, boolean[] used) {
        if(path.size() == nums.length){
            output.add(new ArrayList<>(path));
            return;
        }
        for(int i = 0;i < nums.length; i++){
            if(!used[i]){
                used[i] = true;
                path.add(nums[i]);
                helper(nums, used);
                path.remove(path.size()-1);
                used[i] = false;
            }
        }
    }
}

47.全排列 II

给定一个可包含重复数字的序列 nums ，按任意顺序 返回所有不重复的全排列。

示例 1：

输入：nums = [1,1,2]
输出： [[1,1,2], [1,2,1], [2,1,1]]
示例 2：

输入：nums = [1,2,3]
输出：[[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]
提示：

1 <= nums.length <= 8
-10 <= nums[i] <= 10

解题思路，全排列，所以for循环每次都从0开始，但是要去重上一次取过的数，也就是本身。另外因为数组本身有重复，所以要先排列数组，然后去重重复出现过的数。nums[i] == nums[i-1] && used[i-1]=true;
参数，原来的数组，和used数组。
终止条件：path.size== 3 的时候收集结果。等所有元素遍历完结束。
单层递归逻辑：for循环从0 开始，当used[i]==true 或者 nums[i] == nums[i-1] && used[i-1]=true都continue跳过。

代码实现
class Solution {
    List<List<Integer>> output = new ArrayList<>();
    List<Integer> path = new ArrayList<>();
    public List<List<Integer>> permuteUnique(int[] nums) {
        boolean[] used = new boolean[nums.length];
        Arrays.sort(nums);
        helper(nums, used);
        return output;
    }
    public void helper(int[] nums, boolean[] used){
        if(path.size() == nums.length){
            output.add(new ArrayList<>(path));
            return;
        }
        for(int i = 0; i < nums.length; i++){
            if((i > 0 && nums[i] == nums[i - 1] && used[i-1] == false) || used[i] == true){
                continue;
            }
            
                path.add(nums[i]);
                used[i] = true;
                helper(nums, used);
                path.remove(path.size()-1);
                used[i] = false;
        }
    }
}
