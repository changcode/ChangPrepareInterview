# Remove Duplicates from Sorted Array

Given a sorted array, remove the duplicates in place such that each element appear only once and return the new length.

Do not allocate extra space for another array, you must do this in place with constant memory.

For example,
Given input array nums = [1,1,2],

Your function should return length = 2, with the first two elements of nums being 1 and 2 respectively. It doesn't matter what you leave beyond the new length.


###follow up

Follow up for "Remove Duplicates":
What if duplicates are allowed at most twice?

For example,
Given sorted array nums = [1,1,1,2,2,3],

Your function should return length = 5, with the first five elements of nums being 1, 1, 2, 2 and 3. It doesn't matter what you leave beyond the new length.


第一题，因为是sorted的Array，使用end标记确定最后一个数组位置
O(N)
```java
public int removeDuplicates(int[] nums) {
        int end = 0;
        for (int i = 0; i < nums.length; i++) {
            if(nums[i] == nums[end])
                continue;
            else {
                end++;
                nums[end] = nums[i];
            }
        }
        return end + 1;
    }
```


Follow up：
因为是sorted数组，如果nums[end -1] == nums[i] 可以推出 nums[end - 1] == nums[end] == nums[i] 意味着超过2个同样的数子出现，
```java
public int removeDuplicates(int[] nums) {
        int end = 1;
        for(int i = 2; i < nums.length; i++) {
            if(nums[end - 1] == nums[i]) {
                continue;
            } else {
                end++;
                nums[end] = nums[i];
            }
        }
        return end + 1;
    }
```