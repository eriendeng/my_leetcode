## 无重复字符的最长子串

给定一个字符串，请你找出其中不含有重复字符的 最长子串 的长度。

```
示例 1:

输入: "abcabcbb"
输出: 3 
解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。
```
```
示例 2:

输入: "bbbbb"
输出: 1
解释: 因为无重复字符的最长子串是 "b"，所以其长度为 1。
```
```
示例 3:

输入: "pwwkew"
输出: 3
解释: 因为无重复字符的最长子串是 "wke"，所以其长度为 3。
     请注意，你的答案必须是 子串 的长度，"pwke" 是一个子序列，不是子串。
```

### 解答1

利用哈希表检测字符是否重复，如果重复，则从重复字符的下一个开始计算长度

```
func lengthOfLongestSubstring(s string) int {
    lens := len(s)
    if lens == 0 || lens == 1 {
        return lens
    }
    max := 0							//最长
    tmp_len := 0						//当前循环的长度
    loop := 0							//字符串开始位置
    maps := map[byte]int{}				//哈希表
    for loop<lens {
        if pos, ok := maps[s[loop]]; ok {
            loop = pos + 1
            if tmp_len > max {
                max = tmp_len
            }
            tmp_len = 0
            maps = map[byte]int{}
            continue
        }
        maps[s[loop]] = loop
        tmp_len += 1
        loop += 1
    }
    if tmp_len > max {
        return tmp_len
    }else {
        return max
    }
}
```

GOLANG 484MS 6.8MB，显然是个弟弟提交。。。

### 解答2
看了一下讨论区一位dl的go代码 88.02%, 85.63%<br/>
思考一下，有两个问题。<br/>
在解答1中其实我在字符冲突后做了太多无谓的操作——把不冲突的字符重新数了一遍，这大概就是我耗费大量时间的罪魁祸首。<br/>
使用滑动窗口，就可以有效的解决这个问题，在窗口内部的字符其实我们不用去关心。<br/>
第二个问题，其实我们没有必要使用哈希表去判断冲突的问题，因为字符的出现有先后顺序，我们只需判断新字符和头部字符是否冲突，没有冲突就加进窗口内，有冲突就将窗口开始右滑一位，解决我hash占用大量内存的问题。<br/>
下面是dl的解答：
```
func lengthOfLongestSubstring(s string) int {
    var max  ,start int
	for i := 0 ; i < len(s) ; i ++ {
		for j := start ; j < i ; j++ {
			if s[j] == s[i]{
				start = j + 1
			}
		}

		temp := i - start +1
		if temp > max {
			max = temp
		}
	}
	return max
}
```

GOLANG 12MS 2.6MB