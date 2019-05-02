## 两数之和

给定一个整数数组 nums 和一个目标值 target，请你在该数组中找出和为目标值的那 两个 整数，并返回他们的数组下标。
你可以假设每种输入只会对应一个答案。但是，你不能重复利用这个数组中同样的元素。

示例:
```
给定 nums = [2, 7, 11, 15], target = 9

因为 nums[0] + nums[1] = 2 + 7 = 9
所以返回 [0, 1]
```

### 解答1

简单地将一个数组遍历，找出一个数和它后面的数是否有匹配。

```
func twoSum(nums []int, target int) []int {
    for i:=0; i<len(nums); i++ {
        for j:=i+1; j<len(nums); j++ {
            if nums[i] + nums[j] == target {
                return []int{i, j}
            }
        }
    }
    return []int{0, 1}
}
```

GOLANG 60MS 3MB

### 解答2

看过答案后发现，一趟o(n)的哈希表能够直接解决问题，直接遍历数组并检查对应的target-n是否存在。
利用golang判断map存在 `_, ok := foo["bar"]`

```
func twoSum(nums []int, target int) []int {
	table := map[int]int{}
	for i:=0; i<len(nums); i++ {
		table[nums[i]] = i
	}
	for j:=0; j<len(nums); j++ {
		if _, ok := table[target-nums[j]]; ok {
			if table[target-nums[j]] != j {
                return []int{j, table[target-nums[j]]}
            }
		}
	}
    return []int{0, 1}
}
```

但是其实在逻辑上，map的插入和检查可以放在同一个循环中，因为相加的数字是成对的，所以必定能在后一个中找到前一个的key

```
func twoSum(nums []int, target int) []int {
	table := map[int]int{}
	for j:=0; j<len(nums); j++ {
		table[nums[j]] = j
		if _, ok := table[target-nums[j]]; ok {
			if table[target-nums[j]] != j {
                return []int{j, table[target-nums[j]]}
            }
		}
	}
    return []int{0, 1}
}
```

看了一圈答案，发现大家都是这思路去写的，但是这种方法并不能解决哈希冲突啊。。。在testcase中给出了[3,2,3], 6的这种case，显然会有问题出现。


