## c排序



## 经典十大排序 :

**1. 冒泡排序**

```c++
void MySort(vector<int>& ss)
{
    for(int i = 0; i < ss.size() - 1; ++i){
        for (int j = 0; j < ss.size() - 1 - i; ++j){
            if (ss[j] > ss[j+1]){
                int temp = ss[j];
                ss[j] = ss[j+1];
                ss[j+1] = temp;
            }
        }
    }
}
```

**2. 快速排序**

```c++
void MySort(vector<int>& ss, int l, int r)
{
	if (l>=r) return;
	int i = l, j = r;
	int x = ss[l];
	while(i < j){
		while(i < j && x <= ss[j]){
			j--;
		}
		ss[i] = ss[j];
		while(i < j && x >= ss[i]){
			i++;
		}
		ss[j] = ss[i];
	}
	ss[i] = x;
	MySort(ss,l,i-1);
	MySort(ss,i+1,r);

}
```

**3. 插入排序**

```c++
void MySort(vector<int>& ss)
{
	for(int i = 1; i < ss.size(); ++i){
		int temp = ss[i];
		for(int j = i - 1; j >= 0; --j){
			if (temp < ss[j]){
				ss[j+1] = ss[j];
			} else {
				ss[j+1] = temp;
				break;
			}
			if(j == 0){
				ss[j] = temp;
			}
		}
	}

}
```

**4. 希尔排序**

```c++
void MySort(vector<int>& ss)
{
	for(int i = 1; i < ss.size(); ++i){
		for(int j = i - 1; j >= 0 && ss[j+1]<ss[j]; --j){
			swap(ss[j],ss[j+1]);
		}
	}

}
```

**5. 选择排序**

```c++
void MySort(vector<int>& ss)
{
	for(int i = 0; i < ss.size(); ++i){
		for(int j = i + 1; j < ss.size(); ++j){
			if(ss[j]<ss[i]){
				swap(ss[j],ss[i]);
			}
		}
	}

}
```

**6. 堆排序**

```c++
void Heapify(vector<int>&ss, int begin, int end)
{
	for (int i = 2*begin+1; i < end; i = 2*i + 1){
		if(i+1<end && ss[i]<ss[i+1]){
			++i;
		}
		if(ss[i] > ss[begin]){
			swap(ss[i],ss[begin]);
			begin = i;
		} else{
			break;
		}

	}

}

void MySort(vector<int>& ss)
{
	int len = ss.size();
	for(int i = (len>>1)-1; i >= 0; --i){
		Heapify(ss,i,len);
	}

	for(int i = len - 1; i > 0; --i){
		swap(ss[0],ss[i]);
		Heapify(ss,0,i);
	}

}
```

**7. 归并排序**

```c++
void Merge(vector<int>& ss, int l, int m, int r, vector<int>& temp)
{
	int i = l;
	int j = m + 1;
	int t = 0;
	while(i <= m && j <= r){
		if(ss[i] <= ss[j]) {
			temp[t++] = ss[i++];
		} else {
			temp[t++] = ss[j++];
		}
	} 

	while(i <= m) {
		temp[t++] = ss[i++];
	}

	while(j <= r) {
		temp[t++] = ss[j++];
	}
	t = 0;
	while(l <= r){
		ss[l++] = temp[t++];
	}
}
void MySort(vector<int>& ss, int l, int r, vector<int>& temp)
{
	if(l < r){
		int mid = (l+r)>>1;
		MySort(ss, l, mid, temp);
		MySort(ss, mid+1, r, temp);
		Merge(ss, l, mid, r, temp);
	}
}
```

**8. 计数排序**

```c++
void MySort(vector<int>& ss)
{
	int maxsize = *max_element(ss.begin(),ss.end());
    int minsize = *min_element(ss.begin(),ss.end());
	vector<int> temp(maxsize-minsize+1,0);
	for(const auto& i : ss){
		++temp[i-minsize];
	}
	int t = 0;
	for(int i = 0; i < temp.size() && t < ss.size(); ++i){
		while(temp[i]-->0){
			ss[t++] = i+minsize;
		}
	}
}
```





## 难度: 简单

***



#### [1859. 将句子排序](https://leetcode-cn.com/problems/sorting-the-sentence/)

* 一个 句子 指的是一个序列的单词用单个空格连接起来，且开头和结尾没有任何空格。每个单词都只包含小写或大写英文字母。
* 我们可以给一个句子添加 从 1 开始的单词位置索引 ，并且将句子中所有单词 打乱顺序 。
* 比方说，句子 `This is a sentence` 可以被打乱顺序得到 `sentence4 a3 is2 This1` 或者 `is2 sentence4 This1 a3` 。

* 给你一个 打乱顺序 的句子 s ，它包含的单词不超过 9 个，请你重新构造并得到原本顺序的句子。

* 示例 1：

  ```
  输入：s = "is2 sentence4 This1 a3"
  输出："This is a sentence"
  ```

* 提示

  ```
  2 <= s.length <= 200
  s 只包含小写和大写英文字母、空格以及从 1 到 9 的数字。
  s 中单词数目为 1 到 9 个。
  s 中的单词由单个空格分隔。
  s 不包含任何前导或者后缀空格。
  ```

* 解法一、以单词中数字为条件，分割成单词，并把该数字作为数组下标存储单词

   ```c++
   class Solution {
   public:
       string sortSentence(string s) {
           string::iterator it = s.begin();
           string::iterator wordshead = it;    // 获取单词头部
           vector<string> words(9);
   
           for (; it!=s.end(); ++it)
           {
               if (isdigit(*it)) {             // 判断是否为数字
                   int i = int(*it) - 49;              // 将数字转化为数组下标
                   words[i] = string(wordshead, it); //存储单词
                   words[i].append(" ");
                   if (it != s.end() - 1)
                   {
                       ++it;                           // 跳过空格
                       wordshead = ++it;              
                   }
               }
           }
   
           string ss;
           for(auto w : words)
           {
               ss += w;
           }
   
           ss.erase(ss.end() - 1); // 去掉末尾空格
           
           return ss;
       }
   };
   ```

* 复杂度分析
  * 时间复杂度：O(m)，其中 m 为 s 的长度。遍历字符串、维护单词数组、输出结果的时间复杂度均为 O(m)。
  * 空间复杂度：O(m)，其中 m 为 s 的长度。即为建立并维护单词数组所需的空间。



#### [1370. 上升下降字符串](https://leetcode-cn.com/problems/increasing-decreasing-string/)

* 给你一个字符串 s ，请你根据下面的算法重新构造字符串：

  ```
  从 s 中选出 最小 的字符，将它 接在 结果字符串的后面。
  从 s 剩余字符中选出 最小 的字符，且该字符比上一个添加的字符大，将它 接在 结果字符串后面。
  重复步骤 2 ，直到你没法从 s 中选择字符。
  从 s 中选出 最大 的字符，将它 接在 结果字符串的后面。
  从 s 剩余字符中选出 最大 的字符，且该字符比上一个添加的字符小，将它 接在 结果字符串后面。
  重复步骤 5 ，直到你没法从 s 中选择字符。
  重复步骤 1 到 6 ，直到 s 中所有字符都已经被选过。
  ```

* 在任何一步中，如果最小或者最大字符不止一个 ，你可以选择其中任意一个，并将其添加到结果字符串。

* 请你返回将 s 中字符重新排序后的 结果字符串 。

* 示例 1：

  ```
  输入：s = "aaaabbbbcccc"
  输出："abccbaabccba"
  解释：第一轮的步骤 1，2，3 后，结果字符串为 result = "abc"
  第一轮的步骤 4，5，6 后，结果字符串为 result = "abccba"
  第一轮结束，现在 s = "aabbcc" ，我们再次回到步骤 1
  第二轮的步骤 1，2，3 后，结果字符串为 result = "abccbaabc"
  第二轮的步骤 4，5，6 后，结果字符串为 result = "abccbaabccba"
  ```

* 解法一、哈希建立方式：将0-25下标代码26个字符，存储字符的个数。

  ```c++
  class Solution {
  public:
      string sortString(string s) {
          vector<int> ctr(26);
          const int size = s.length();
          for (char& c : s)
          {
              ctr[c-'a']++;
          }
          string ss;
  
          while (ss.length() < size)
          {
              for (int i = 0; i < 26; ++i)
              {
                  if (ctr[i]) {
                      --ctr[i];
                      ss.push_back('a'+ i);
                  }
              }
              
              for (int i = 25; i >= 0; --i)
              {
                  if (ctr[i]) {
                      --ctr[i];
                      ss.push_back('a'+ i);
                  }
              }
  
          }
          return ss;
      }
  };
  ```

* 复杂度分析
  * 时间复杂度：O(∣Σ∣×∣s∣)，其中∣Σ∣为字符集，s 为传入的字符串，在这道题中，字符集为全部小写字母，∣Σ∣=26。最坏情况下字符串中所有字符都相同，那么对于每一个字符，我们需要遍历一次整个桶。

  * 空间复杂度：O(∣Σ∣)，其中 Σ为字符集。我们需要和 ∣Σ∣ 等大的桶来记录每一类字符的数量





#### [1528. 重新排列字符串](https://leetcode-cn.com/problems/shuffle-string/)

* 给你一个字符串 s 和一个 长度相同 的整数数组 `indices` 。

* 请你重新排列字符串 s ，其中第 i 个字符需要移动到 `indices[i]` 指示的位置。

* 返回重新排列后的字符串。

* 示例 1:

  ```
  输入：s = "codeleet", indices = [4,5,6,7,0,2,1,3]
  输出："leetcode"
  解释：如图所示，"codeleet" 重新排列后变为 "leetcode" 。
  ```

* 提示：

  ```
  s.length == indices.length == n
  1 <= n <= 100
  s 仅包含小写英文字母。
  0 <= indices[i] < n
  indices 的所有的值都是唯一的（也就是说，indices 是整数 0 到 n - 1 形成的一组排列）。
  ```

* 解法一、使用字符串存储排序后的值，`indices`的值作为`ss`的下标，`indices`的下标作为`s`的下标

  ```c++
  class Solution {
  public:
      string restoreString(string s, vector<int>& indices) {
          const int size = indices.size();
          string ss(size,0);
          for (int i = 0; i < size; ++i)
          {
              ss[indices[i]] = s[i];
          }
  
          return ss;
      }
  };
  ```

* 复杂度分析

  * 时间复杂度：O(N)，其中 N 为字符串 s 的长度。我们只需对字符串 s 执行一次线性扫描即可。
  * 空间复杂度：O(N)。其中 N 为字符串 s 的长度。

* 解法二、使用swap函数交换字符

  ```c++
  class Solution {
  public:
      string restoreString(string s, vector<int>& indices) {
          const int size = indices.size();
          for (int i = 0; i < size; i++) {
              while (indices[i] != i) {
                  swap(s[i], ss[indices[i]]); 
                  swap(indices[i], indices[indices[i]]); 
              }
          }
      
          return s;
      }
  };
  ```

* 复杂度分析

  * 时间复杂度：O(M)，其中M 为传入的字符串。尽管代码看上去有两层循环，但因为不会处理相同的封闭路径，每个下标实际上只被处理了一次，故时间复杂度是线性的
  * 空间复杂度：O(1)。我们只需开辟常量大小的额外空间





#### [349. 两个数组的交集](https://leetcode-cn.com/problems/intersection-of-two-arrays/)

* 给定两个数组，编写一个函数来计算它们的交集。

 

* 示例 1：

  ```
  x 输入：nums1 = [1,2,2,1], nums2 = [2,2]输出：[2]
  ```

* 说明：

  ```
  输出结果中的每个元素一定是唯一的。
  我们可以不考虑输出结果的顺序
  ```

* 解法一、sort排序+双指针

  ```c++
  class Solution {
  public:
      vector<int> intersection(vector<int>& nums1, vector<int>& nums2) {
          sort(nums1.begin(), nums1.end());
          sort(nums2.begin(), nums2.end());
  
          int size1 = nums1.size();
          int size2 = nums2.size();
  
          vector<int> ii;
          int s1 = 0, s2 = 0;
          while(s1 < size1 && s2 < size2)
          {   
              if(nums1[s1] == nums2[s2]){
                  if (ii.empty() || ii.back() != nums1[s1]){
                      ii.push_back(nums1[s1]);
                  }
                  ++s1;
                  ++s2;
              } else if (nums1[s1] > nums2[s2]) {
                  ++s2;
              } else if (nums1[s1] < nums2[s2]) {
                  ++s1;
              }
          }
          return ii;
      }
  };
  ```

* 复杂度分析

  * 时间复杂度：`O(mlog⁡m+nlog⁡n)`，其中 m 和 n 分别是两个数组的长度。对两个数组排序的时间复杂度分别是`O(mlogm) 和 O(nlog⁡n)`，双指针寻找交集元素的时间复杂度是 O(m+n)，因此总时间复杂度是 `O(mlogm+nlogn)`。
  * 空间复杂度：`O(log⁡m+log⁡n)`，其中 `m` 和 `n` 分别是两个数组的长度。空间复杂度主要取决于排序使用的额外空间。

* 解法二、哈希表+递归

  ```c++
  class Solution {
  public:
      vector<int> intersection(vector<int>& nums1, vector<int>& nums2) {
          unordered_set<int> set1, set2;
          for (auto& num : nums1) {
              set1.insert(num);
          }
          for (auto& num : nums2) {
              set2.insert(num);
          }
          return getIntersection(set1, set2);
      }
  
      vector<int> getIntersection(unordered_set<int>& set1, unordered_set<int>& set2) {
          if (set1.size() > set2.size()) {
              return getIntersection(set2, set1);
          }
          vector<int> intersection;
          for (auto& num : set1) {
              if (set2.count(num)) {
                  intersection.push_back(num);
              }
          }
          return intersection;
      }
  };
  ```

  

#### [1356. 根据数字二进制下 1 的数目排序](https://leetcode-cn.com/problems/sort-integers-by-the-number-of-1-bits/)

* 给你一个整数数组 arr 。请你将数组中的元素按照其二进制表示中数字 1 的数目升序排序。

* 如果存在多个数字二进制中 1 的数目相同，则必须将它们按照数值大小升序排列。

* 请你返回排序后的数组。

* 示例 1：

  ```
  输入：arr = [0,1,2,3,4,5,6,7,8]
  输出：[0,1,2,4,8,3,5,6,7]
  解释：[0] 是唯一一个有 0 个 1 的数。
  [1,2,4,8] 都有 1 个 1 。
  [3,5,6] 有 2 个 1 。
  [7] 有 3 个 1 。
  按照 1 的个数排序得到的结果数组为 [0,1,2,4,8,3,5,6,7]
  ```

* 提示：

  ```
  1 <= arr.length <= 500
  0 <= arr[i] <= 10^4
  ```

* 解法一、把1-10000的值的1的个数储存起来放在vector，再根据要求编写sort条件

  ```c++
  class Solution {
  public:
      vector<int> sortByBits(vector<int>& arr) {
              vector<int> ss(10001, 0);
              for(int i = 1; i < 10001; ++i)
              {
                  ss[i] = ss[i>>1] + (i & 1);
              }
  
              sort(arr.begin(), arr.end(), [&](int x,int y){
                  if (ss[x] == ss[y])
                      return x < y;
                  return ss[x] < ss[y];
              });
              return arr;
      }
  };	
  ```

* 复杂度分析
  * 时间复杂度：`O(nlog⁡n)`，其中 `n` 为整数数组 `arr` 的长度。
  * 空间复杂度：`O(n)`，其中 `n` 为整数数组 `arr` 的长度。



* 解法二、利用循环>>1位，计算每个数的1的个数，然后排序

  ```c++
  class Solution {
  public:
      int get(int m)
      {
          int count = 0;
          do{
              ++count;
          }while(m &= (m-1));
          return count;
      }
      vector<int> sortByBits(vector<int>& arr) {
              sort(arr.begin(), arr.end(), [&](int x,int y){
                  int num1 = get(x);
                  int num2 = get(y);
                  if (num1 == num2)
                      return x < y;
                  return num1 < num2;
              });
              return arr;
      }
  };
  ```

* 解法三、使用pair<int,int>存储arr数字的1的个数，再排序,然后取value的值

  ```c++
  class Solution {
  public:
      vector<int> sortByBits(vector<int>& arr) {
              vector<int> ss;
              vector<pair<int ,int> > onecount;
              const int len = arr.size();
              for(int i = 0; i < len; ++i)
              {
                  int count = 0;
                  int x = arr[i];
                  do{
                      ++count;
                  } while (x &= (x-1));
                  onecount.push_back(pair<int, int>(count, arr[i]));
              }
  
              sort(onecount.begin(), onecount.end());
              vector<pair<int, int> >::iterator it = onecount.begin();
              for (int i = 0; it != onecount.end(); ++it)
              {
                  ss.push_back(it->second);
              }
              return ss;
      }
  };
  ```








#### [1636. 按照频率将数组升序排序](https://leetcode-cn.com/problems/sort-array-by-increasing-frequency/)

* 给你一个整数数组 `nums` ，请你将数组按照每个值的频率 **升序** 排序。如果有多个值的频率相同，请你按照数值本身将它们 **降序** 排序。 

* 请你返回排序后的数组。

* 示例 1：

  ```
  输入：nums = [1,1,2,2,2,3]
  输出：[3,1,1,2,2,2]
  解释：'3' 频率为 1，'1' 频率为 2，'2' 频率为 3 。
  ```

* 提示：

  ```
  1 <= nums.length <= 100
  -100 <= nums[i] <= 100
  ```

* 解法一、模拟哈希表+排序

  ```c++
  class Solution {
  public:
      vector<int> frequencySort(vector<int>& nums) {
          sort(nums.begin(),nums.end());
          int ss[201] = {0};
          for(int i = 0; i < nums.size(); ++i)
          {
              int count = 1;
              while(i < nums.size()-1 && nums[i] == nums[i+1])
              {
                  ++count;
                  ++i;
              }
              ss[nums[i]+100] = count;
          }
          sort(nums.begin(), nums.end(), [&](int x, int y)
          {
              if(ss[x+100] == ss[y+100])
                  return x > y;
              return ss[x+100] < ss[y+100];
          });
          
          return nums;
      }
  };
  ```






#### [1710. 卡车上的最大单元数](https://leetcode-cn.com/problems/maximum-units-on-a-truck/)

* 请你将一些箱子装在 一辆卡车 上。给你一个二维数组 `boxTypes` ，其中 `boxTypes[i]` = `[numberOfBoxesi, numberOfUnitsPerBoxi] `：
* `numberOfBoxesi` 是类型 i 的箱子的数量。
* `numberOfUnitsPerBoxi` 是类型 i 每个箱子可以装载的单元数量。

* 整数 `truckSize` 表示卡车上可以装载 箱子 的 最大数量 。只要箱子数量不超过 `truckSize` ，你就可以选择任意箱子装到卡车上。返回卡车可以装载 单元 的 最大 总数。

* 示例 1：

  ```
  输入：boxTypes = [[1,3],[2,2],[3,1]], truckSize = 4
  输出：8
  解释：箱子的情况如下：
  - 1 个第一类的箱子，里面含 3 个单元。
  - 2 个第二类的箱子，每个里面含 2 个单元。
  - 3 个第三类的箱子，每个里面含 1 个单元。
    可以选择第一类和第二类的所有箱子，以及第三类的一个箱子。
    单元总数 = (1 * 3) + (2 * 2) + (1 * 1) = 8
  ```

* 解法一、贪心算法, 注意`const auto&:`(使用引用无需拷贝,效率大增)

  ```c++
  class Solution {
  public:
      int maximumUnits(vector<vector<int>>& boxTypes, int truckSize) {
          sort(boxTypes.begin(), boxTypes.end(), [&](const auto& a, const auto& b){
              return a[1] > b[1];
          });
          int ss = 0;
          for (int i = 0; i < boxTypes.size() && truckSize > 0; ++i)
          {
              int n = min(truckSize, boxTypes[i][0]);
              truckSize-=n;
              ss += n * boxTypes[i][1];
          }
          return ss;
      }
  };
  ```

  



#### [1640. 能否连接形成数组](https://leetcode-cn.com/problems/check-array-formation-through-concatenation/)

* 给你一个整数数组 arr ，数组中的每个整数 互不相同 。另有一个由整数数组构成的数组 pieces，其中的整数也 互不相同 。请你以 任意顺序 连接 pieces 中的数组以形成 arr 。但是，不允许 对每个数组 pieces[i] 中的整数重新排序。

* 如果可以连接 pieces 中的数组形成 arr ，返回 true ；否则，返回 false 。

* 示例 1：

  ```
  输入：arr = [91,4,64,78], pieces = [[78],[4,64],[91]]
  输出：true
  解释：依次连接 [91]、[4,64] 和 [78]
  ```

* 提示：

  ```
  1 <= pieces.length <= arr.length <= 100
  sum(pieces[i].length) == arr.length
  1 <= pieces[i].length <= arr.length
  1 <= arr[i], pieces[i][j] <= 100
  arr 中的整数 互不相同
  pieces 中的整数 互不相同（也就是说，如果将 pieces 扁平化成一维数组，数组中的所有整数互不相同）
  ```

* 解法一、unordered_map记录

  ```c++
  class Solution {
  public:
      bool canFormArray(vector<int>& arr, vector<vector<int>>& pieces) {
          unordered_map<int, int> ss; 
          for (int i = 0; i < pieces.size(); ++i) { 
              ss[pieces[i][0]] = i; 
          } 
          
          for (int i = 0; i < arr.size();) 
          { 
              if (ss.find(arr[i]) == ss.end()) 
                  return false; 
                  
              auto& p = pieces[ss[arr[i]]]; 
              for (int j = 0; j < p.size(); ++i,++j) 
              { 
                  if (arr[i] != p[j]) 
                      return false; 
              } 
          } 
          return true;
  
      }
  };
  ```

* 解法二、直接对比

  ```c++
  class Solution {
  public:
      bool canFormArray(vector<int>& arr, vector<vector<int>>& pieces) {
  
          for (int i = 0; i < pieces.size(); ++i)
          {
              int n = pieces[i].size();
              int j = 0;
              for (; j < (arr.size() - n + 1); ++j)
              {
                  if (arr[j] == pieces[i][0])
                  {
                      for (int k = 1; k < n; ++k)
                      {
                          if (j+k < arr.size() && arr[j+k] != pieces[i][k])
                          {
                              return false;
                          }
                      }  
                      break;
                  }
                  
              }
              if (j >= (arr.size() - n + 1))
                  return false;
          }
          return true;
      }
  };
  ```

  



## 难度: 中等

***

#### [1329. 将矩阵按对角线排序](https://leetcode-cn.com/problems/sort-the-matrix-diagonally/)

* 矩阵对角线 是一条从矩阵最上面行或者最左侧列中的某个元素开始的对角线，沿右下方向一直到矩阵末尾的元素。例如，矩阵 mat 有 6 行 3 列，从 mat[2][0] 开始的 矩阵对角线 将会经过 mat[2][0]、mat[3][1] 和 mat[4][2] 。

* 给你一个 m * n 的整数矩阵 mat ，请你将同一条 矩阵对角线 上的元素按升序排序后，返回排好序的矩阵。

* 实例一:

  ```
  输入：mat = [[3,3,1,1],[2,2,1,2],[1,1,1,2]]
  输出：[[1,1,1,1],[1,2,2,2],[1,2,3,3]]
  ```

* 提示：

  ```
  m == mat.length
  n == mat[i].length
  1 <= m, n <= 100
  1 <= mat[i][j] <= 100
  ```

* 解法一、按顺序获取值,然后使用希尔排序

  ```c++
  class Solution {
  public:
      vector<vector<int>> diagonalSort(vector<vector<int>>& mat) { 
  
          for(int i = 0; i < mat.size(); ++i){
              for(int j = i + 1,t = 1; j < mat.size()&&t < mat[j].size(); ++j,++t) {
                  for(int m = j-1, n = t-1;m >= 0&&n >= 0;--m,--n){
                      if (mat[m+1][n+1] < mat[m][n])
                      {
                          swap(mat[m+1][n+1],mat[m][n]);
                      }
                  }
              }
          }
  
          for(int i = 0; i < mat[0].size(); ++i){
              for(int j = i+1, t = 1; j < mat[0].size() && t < mat.size(); ++j,++t){
                  for(int m = t-1, n = j-1;m >= 0&&n >= 0;--m,--n){
                      if (mat[m+1][n+1] < mat[m][n])
                      {
                          swap(mat[m+1][n+1],mat[m][n]);
                      }
                  }
              }
          }
          return mat;
      }
  };
  ```

* 解法二、根据直线公式y-x=b(45度角k==1), 用unordered_map把同一对角线的值存取起来,再进行排序

  ```c++
  class Solution {
  public:
      vector<vector<int>> diagonalSort(vector<vector<int>>& mat) { 
          unordered_map<int, vector<int> > ss;
          for(int i = 0; i < mat.size(); ++i){
              for(int j = 0; j < mat[0].size(); ++j) {
                  ss[j-i].push_back(mat[i][j]);
              }
          }
  
          for(auto& x : ss){
              sort(x.second.rbegin(),x.second.rend());
          }
  
          for(int i = 0; i < mat.size(); ++i){
              for(int j = 0; j < mat[0].size(); ++j) {
                  mat[i][j] = ss[j-i].back();
                  ss[j-i].pop_back();
              }
          }
          return mat;
      }
  };
  ```

  