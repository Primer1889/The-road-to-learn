# The-road-to-learn 取经之路就在眼前 -- 算法

实习生上岸,每一轮面试至少有那么一道算法题,对算法的考察是一个综合能力的体现,务必坚持刷算法题

<details>
<summary>两数之和</summary>
  给定一个整数数组 nums 和一个目标值 target，请你在该数组中找出和为目标值的那 两个 整数，并返回他们的数组下标</br>
  <pre><code>
    class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
  
        //int: 值  int:下标
        map<int,int> m;
        int index=0;
        
        //先把之存入map映射
        for(auto val:nums){
            m[val]=index;
            index++;
        }

        /*
            1.根据key=target-num[i]  判断是否存在这个数使其相加等于target
            2.判断两个数的下标不相同才行 m[key]!=i
            3.返回值的第一个数是 i,第二个数才是找出来的那个key的下标
        */
        vector<int> result(2,0);
        for(int i = 0 ; i < nums.size();i++){
            int key = target-nums[i];
            if(m.find(key)!=m.end() && m[key] != i){
                result[0]=i;
                result[1]=m[key];
                return result;
            }
        }
        return vector<int>();
    }
};
  </code></pre>
  时间复杂度分析:  O(n)
  
  扩展知识: c++ map 判断key是否存在 1. map.find(key) !=map.end() 2. map.count(key) != 0
</details>

<details>
<summary>主要元素</summary>
  如果数组中多一半的数都是同一个，则为主要元素。给一个数组，找到主要元素。若没有，返回-1</br>
 一种可能超时的做法
 扩展知识: std:count(v.begin(),v.end(),val); 统计val的个数
 <pre><code>
    class Solution {
public:
    int majorityElement(vector<int>& nums) {
        int len = nums.size()/2;
        for(auto val:nums){
            if(count(nums.begin(),nums.end(),val) >len){
                return val;
            }
        }

        return -1;
    }
};
  </code></pre>
  
  也是一种超时的做法set
  扩展知识: multiset元素可以重复的集合
  <pre><code>
    class Solution {
public:
    int majorityElement(vector<int>& nums) {
        int len = nums.size()/2;
        multiset<int> s;

        for(auto val:nums){
            s.insert(val);
        }

         for(auto val:nums){
            if(s.count(val)>len){
                return val;
            }
        }


        return -1;
    }
};
  </code></pre>
  
  利用中位数特性
   <pre><code>
    class Solution {
public:
    int majorityElement(vector<int>& nums) {
        int len = nums.size()/2;
        //先排序
        sort(nums.begin(),nums.end());
        //利用中位数特性:元素个数多以数组长度一半的数必定出现在数组中间及其以上
        if(count(nums.begin(),nums.end(),nums[len]) > len){
            return nums[len];
        }

        return -1;
    }
};
  </code></pre>
  
  时间复杂度分析:  O(n)
  
  扩展知识: c++ map 判断key是否存在 1. map.find(key) !=map.end() 2. map.count(key) != 0
</details>



<details>
<summary>消失的数字</summary>
  数组nums包含从0到n的所有整数，但其中缺了一个。请编写代码找出那个缺失的整数</br>
  <pre><code>
    class Solution {
public:
    int missingNumber(vector<int>& nums) {
        //等差数列求和,计算 0~n的和  公式: n*a1+[n*d*(n-1)]/2
        int n = nums.size();
        int sum = 1*n+(n*1*(n-1))/2;
        //计算现有数组的总和  结果就是:两者相减
        return sum-accumulate(nums.begin(),nums.end(),0);

    }
};
  </code></pre>
  
  扩展知识: accumulate计算集合的和
</details>



<details>
<summary>位运算相加</summary>
  不适用+号</br>做a+b的结果
  <pre><code>
    class Solution {
public:
    int add(int a, int b) {
        //当没有进位的时候就退出
        while(b){
            //相加
            unsigned long tmp = a^b;
            //进位
            b = (unsigned long )(a&b)<<1;
            a = tmp;
        }

        return (int)a;
    }
};
  </code></pre>
  
  为例满足条件"不使用+号",再写一种方法,效率不考虑,主要是多一种实现的方式
  <pre><code>
   class Solution {
public:
    int add(int a, int b) {
        vector<int> v(2,0);
        v[0]=a;
        v[1]=b;
        return accumulate(v.begin(),v.end(),0);
    }
};
  </code></pre>
  
</details>


<details>
<summary>阶乘尾部0的个数</summary>
  其实就是不断除以5,统计5的个数
  <pre><code>
   class Solution {
public:
    int trailingZeroes(int n) {
        std::ios::sync_with_stdio(false);
        int len5 = 0;
        while(n>0){
            len5+=n/5;
            n/=5;
        }

        return len5;
    }
};
  </code></pre>
</details>


<details>
<summary>汉诺塔</summary>
  A->B移动   B->C移动   //从A中删除  从C中添加
  <pre><code>
   class Solution {
public:
    void hanota(vector<int>& A, vector<int>& B, vector<int>& C) {
        move(A.size(),A,B,C);
    }

    //A->B    B-C  C增加就push,数据来源方就要删除
    void move(int len,vector<int>& a,vector<int>& b,vector<int>& c){
        if(len == 1){
            c.push_back(a.back());
            a.pop_back();
            return;
        }

        move(len-1,a,c,b);
        c.push_back(a.back());
        a.pop_back();
        move(len-1,b,a,c);
    }
};
  </code></pre>
</details>


<details>
<summary>合并有序数组(每个数组去前几个元素)</summary>
  一切尽在注释,就是速度有点慢
  <pre><code>
   class Solution {
public:
    void merge(vector<int>& A, int m, vector<int>& B, int n) {
        //先删除
        A.erase(A.begin()+m,A.end());
        //再插入
        A.insert(A.begin()+m,B.begin(),B.begin()+n);
        //最后排序
        sort(A.begin(),A.end());
    }
};
  </code></pre>
  
  三指针法则,两个指针分别指向两个数组的最后一个元素,还有一个指针指向总长度m+n处
  <pre>
  <code>
  class Solution {
public:
    void merge(vector<int>& A, int m, vector<int>& B, int n) {
        std::ios::sync_with_stdio(false);
        int totalLen = m+n-1;
        int indexA = m-1;
        int indexB = n-1;

        while(indexA >=0 && indexB >= 0){
            if(A[indexA] > B[indexB]){
                A[totalLen--] = A[indexA--];
            }else{
                A[totalLen--] = B[indexB--];
            }
        }

        while(indexA >=0){
            A[totalLen--] = A[indexA--];
        }
        while(indexB >= 0){
            A[totalLen--] = B[indexB--];
        }
    }
};
  </code>
  </pre>
  
</details>


<details>
<summary>连续数组最大值</summary>
  简单的dp
  <pre><code>
   class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        if(nums.size()==0)return 0;
        if(nums.size()==1)return nums[0];
        //初始化
        int maxVal = nums[0];
        for(int i = 1; i < nums.size();i++){
            //前一个是正数自然是增加
            if(nums[i-1] > 0){
                nums[i]+=nums[i-1];
            }
            //保留最大值
            if(nums[i]>maxVal){
                maxVal = nums[i];
            }
        }

        return maxVal;
    }
};
  </code></pre>
</details>



<details>
<summary>模板</summary>
  内容
  <pre><code>
   
  </code></pre>
</details>



<details>
<summary>模板</summary>
  内容
  <pre><code>
   
  </code></pre>
</details>


<details>
<summary>模板</summary>
  内容
  <pre><code>
   
  </code></pre>
</details>


<details>
<summary>模板</summary>
  内容
  <pre><code>
   
  </code></pre>
</details>



<details>
<summary>模板</summary>
  内容
  <pre><code>
   
  </code></pre>
</details>


<details>
<summary>模板</summary>
  内容
  <pre><code>
   
  </code></pre>
</details>



<details>
<summary>模板</summary>
  内容
  <pre><code>
   
  </code></pre>
</details>
