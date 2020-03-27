# 找出数组中重复的数字
## 描述
在一个长度为 n 的数组 nums 里的所有数字都在 0～n-1 的范围内。数组中某些数字是重复的，但不知道有几个数字重复了，也不知道每个数字重复了几次。请找出数组中任意一个重复的数字。

## 示例
```shell
输入：
[2, 3, 1, 0, 2, 5, 3]
输出：2 或 3 
```

## 限制
`2 <= n <= 100000`

# Java解法
## 1.最初的想法
循环嵌套，外层循环遍历整个数组，内存循环遍历当前值以后的值以寻找重复元，若找到重复元则返回当前值。
```
class Solution3 {
    public int findRepeatNumber(int[] nums) {
        for(int i = 0; i <= nums.length - 1; i++){
            for(int j = i + 1; j <=nums.length - 1; j++){
                if(nums[i] == nums[j]){
                    return nums[i];
                }
                else{
                    continue;
                }
            }
        }
        return -1;
    }
}
```

## 2.空间换时间的想法
设置一个初值全零的数组，数组大小与输入数组相同，用以标记原数组各个数字出现次数，出现一次就对相应index处的元素+1.每访问一个元素就判断index处是否为0，不为0则表示重复出现，立即返回该值。
```
class Solution3_1 {
    public int findRepeatNumber(int[] nums){
        int[] temp = new int[nums.length];
        for(int num: nums){
            if(temp[num] > 0){
                return num;
            }
            else {
                temp[num]++;
            }
        }
        return -1;
    }
}
```
>该方法还可以用于统计数组中每个元素出现次数。

## 3.使用迭代器
将输入数组转换为List\<Integer>类型，在实例化List类型的迭代器来遍历，基本思想与方法2相同。
```
class Solution3_2 {
    public int findRepeatNumber(int[] nums){
        List<Integer> list = Arrays.stream(nums).boxed().collect(Collectors.toList());
        // Arrays.stream(arr) 可以替换成IntStream.of(arr)。
        // 1.使用Arrays.stream将int[]转换成IntStream。
        // 2.使用IntStream中的boxed()装箱。将IntStream转换成Stream<Integer>。
        // 3.使用Stream的collect()，将Stream<T>转换成List<T>，因此正是List<Integer>。

        int[] temp = new int[nums.length];
        for(Iterator it = list.iterator(); it.hasNext();){  //初始化list的迭代器，用于后面访问list实例内部元素
            int i = (Integer) it.next();
            if(temp[i] > 0){
                return i;
            }
            else {
                temp[i]++;
            }
        }
        return -1;
    }
}
```

## 4.官方答案
使用集合存储已经遇到的数字，如果遇到的一个数字已经在集合中，则当前的数字是重复数字(利用Set类型中元素不可重复的性质)。
* 初始化集合为空集合，重复的数字 repeat = -1
* 遍历数组中的每个元素：
  * 将该元素加入集合中，判断是否添加成功
    * 如果添加失败，说明该元素已经在集合中，因此该元素是重复元素，将该元素的值赋给 repeat，并结束遍历
```
class Solution {
    public int findRepeatNumber(int[] nums) {
        Set<Integer> set = new HashSet<Integer>();
        int repeat = -1;
        for (int num : nums) {
            if (!set.add(num)) {
                repeat = num;
                break;
            }
        }
        return repeat;
    }
}
```
[作者：LeetCode-Solution](https://leetcode-cn.com/problems/shu-zu-zhong-zhong-fu-de-shu-zi-lcof/solution/mian-shi-ti-03-shu-zu-zhong-zhong-fu-de-shu-zi-b-4/)
