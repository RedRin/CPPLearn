# vector
### erase()
	 iterator erase ( iterator position );
	 iterator erase ( iterator first, iterator last );
> 删除后 size 会改变，因此要重新计算，不能简单的 ```for(;i < vec.size();)```

	while (ite != students.end()) {    //students.end() 要重新计算
		if (fgrade(*ite)) {
			fail.push_back(*ite);
			ite = students.erase(ite); // 返回指向被删除元素下一个位置的迭代器
			//被删除位置之后的迭代器会无效
		}
		else {
			++ite;
		}
	}

### insert()
	void insert ( iterator position, InputIterator first, InputIterator last );
	eg: ret.insert(ret.end(), bottom.begin(), bottom.end());

### reverse(n)
保留空间以保存n个元素，但不对这些元素进行初始化。这个操作不会改变容器的大小。它仅仅会影响向量为了响应对insert或push_back的重复调用而分配内存的频率

### resize(n)
给vector一个新长度，这个长度等于n。如果n比当前长度小，那么，在这个向量中位于位置n之后的元素会被删除掉。如果n比当前长度大，那新的元素（初始化为默认值）会被添加到向量中。

# list
不能用标准库的sort函数来排序list,而要调用它自己的sort方法   
list的迭代器没有定义 `+` 运算符，<font color=red>因此不能 `iter+n` 操作</font>   
但支持 `++` , `--` 来移到上下一个元素。

	sort()
	sort(cmp)
> 使用适用于list元素类型的 `<` 运算符来排列元素，或者使用判定cmp
> 来排列元素

例子：

	list<Student_info> students;
	students.sort(compare);



# map
映射表，一种支持高效<font color=red>**查询**</font>的`关联`容器   
自动排序的，无法对它进行排序操作，因此很多库中的排序算法对它无效   
被声明为const的map没有 `[]` 运算符，因为`[]` 访问没有的键时会导致构造一个默认的键值对，这与 const map 不符

	string s;
	map<string, int> counters;   //一个从字符串到int的映射表
	while(cin>>s)
		++counters[s];
	for(map<string, int>::const_iterator it=counters.begin();it!=counters.end();++it) {
		cout << it->first << "\t" << it->second << endl;
	}
> 一种计算输入中每个单词出现次数的方法   
> 如果用一个未曾出现过的键来作为索引，那么映射表会自动创建一个具有这个键的元素，值被初始化为默认值。

> <font color=red> C++的 `map` 比最好的散列表数据结构速度稍慢，但是使用方便，而且会自动排序，散列表还要设计一个好的散列函数，</font>  
> map底层实现为平衡树

### map<K, V> m(cmp)
使用判定 `cmp` 来确定元素顺序的映射表

### find()
查找指定键的值，返回指向这个键值对的迭代器

	map<string, int>::const_iterator it = tempMap.find("hello");
	cout << it->first << "--->" << it->second << endl;
> 找不到时返回 tempMap.end()

## pair
与映射表相互关联的库类型，用来同时接触键和关联的值   
保存了两个分别叫做 `first` 和 `second` 的元素   
映射表的每一个元素都是一个数对

对于一个键类型为K而值类型为V的映射表来说，关联的pair类型是pair<const K, V>, 注意到这里是const，因此键的内容不会被修改



# <string\>

### getline
	istream& getline ( istream& is, string& str );
> 返回 ```is```

### 构造string(iterator first, iterator last)

### wstring
<font color=red>宽字符版</font>的string

	wstring wstr = "你好 ";
	wcout << wstr;
> 配合着 `wcout` 来使用

# iterator
> `*` 的优先级比 `.` 运算符低， `*ite.name` 会被解释为 `*(ite.name)`   
> 而与 `++` 和 `--` 相同优先级， `*ite++` 等价于 `*(ite++)` 解释为 先取 `*ite` 然后对 <font color=red>`ite`</font> 加一

	while (ite != retCur.end()) {
		cout << *ite++ << endl;
	}  //遍历

> `->` 和 `.` 运算符优先级相同， `ite->stu.name` 解释为 `ite->stu 的 name`

<font color=red> 如果一个容器支持索引，那么它的迭代器也会支持</font>

	string::iterator ite = string("hello").end();
	beg[-1];    //等价于*(beg-1)

	i[sep.size()];  //等价于*(i+sep.size())


### const_iterator
const指的是不能改变指向的容器中的值，但是可以改变指向

# <cctype\>
为处理字符数据提供了有用的函数

	isspace(c)
	isalpha(c)  //是否字母
 	isdigit(c)
	isalnum(c)  //是否字母或数字
	ispunct(c)  //是否标点符号
	isupper(c)
	islower(c)
	toupper(c)
	tolower(c)

# <algorithm\>
<font color=red>**这些算法函数 经过了优化，效率很高**</font>

### sort()
只有 `vector` 和 `string` 支持

### stable_sort()

![](imgs/sort.png)

### copy()

	copy(bottom.begin(), bottom.end(), back_inserterer(ret));
	等价于
	ret.insert(ret.end(), bottom.begin(), bottom.end());

> copy是一个泛型算法的例子， back_inserter()是一个迭代器适配器(定义在<iterator\>）

### find_if()
	find_if ( InputIterator first, InputIterator last, Predicate pred );
> Returns an iterator to the first element in the range [first,last) for which applying pred to it, is true.   
> 这个 pred 是一个自定义的 bool 函数， 它的参数类型是指定的容器的元素的类型，如string要填char  
> <font color=red>不能与 const_iterator 搭配 </font>   
> 失败时返回 `last`

### equal()

	bool equal ( InputIterator1 first1, InputIterator1 last1,
               InputIterator2 first2 );
	例子： 回文的一种计算方法
	return equal(s.begin(), s.end(), s.rbegin());
> 头两个迭代器指示了第一个序列，第三个参数则是第二个序列的起点。equal<font color=red>假设</font>第二个序列的长度足以容纳第一个序列的长度，因此不需要结尾迭代器

### find()

	static const string url_ch = "~;/?:@=&$-_.+!*`(),'";
	char c='=';
	find(url_ch.begin(), url_ch.end(), c)  

> 从url_ch的开始到结尾查询字符c，找到了就返回指示它位置的迭代器，否则返回第二个参数url_ch.end()

### search()

	static const string sep = "://";
	iter i = search(i, e, sep.begin(), sep.end());
> 在前一对迭代器指定的范围内，查找第二对迭代器指定的一串数据，找到了就返回指向这串数据第一个字符的迭代器，否则返回 `e`

### remove_copy()

	vector<double> nonzero;
	remove_copy(s.homework.begin(), s.homework.end(), back_inserter(nonzero), 0);
> remove_copy函数查找与一个特定值匹配的所有值并把这些值从容器中“删除”掉。在输入序列中所有不被“删除”的值将被复制到目的地。  
> 这里的删除不是指传进来的容器，传来的容器<font color=red>不受影响</font>

### remove\_copy\_if()

	remove_copy_if(students.begin(), students.end(), back_inserter(fail), pgrade);   

	bool pgrade(const Student_info& s) { return grade(s)>=60;}
> 类似remove_copy 只不过，这里是把满足pgrade的去掉，再复制剩余的

### remove_if()
“删除”满足谓词的所有元素

	students.erase(remove_if(students.begin(),students.end(),fgrade), students.end());

	bool fgrade(const Student_info& s) {return grade(s)<60;}

### remove(b, e, t)
作用与remove_if一样，但是是把等于t的元素“删除”

> remove_if“删除”的机制是把不满足谓词函数的都复制到容器的<font color=red>开头位置</font>,  
> 返回指向最后一个不被“删除"的数据的<font color=red>后一个</font>位置的迭代器

![](imgs/remove_if1.png)

![](imgs/remove_if2.png)


### transform()

	transform(students.begin(), students.end(), back_inserter(grades), grade);
> 前两个迭代器指定了范围，一个输入序列  
> 第三个迭代器指示了一个目的地，它将保存第四个参数函数的运行结果，要保证目的地有足够的空间，这里用插入迭代器就肯定够空间了  
> 第四个参数是函数指针，transform将这个函数应用到输入序列的每一
个元素中

![](imgs/transform.png)


### partition() 和 stable_partition()
以一个序列作为参数并重新排列序列的元素，以使满足谓词的元素排在那些不满足的元素之前。    

partition可能会破坏原先的顺序，而stable_partition会让各区域的元素的相互位置保持不变。     
如：对按字母排好序了的学生姓名向量操作时。

返回第二区域的第一个元素的位置（迭代器）

	vector<double>::iterator iter = stable_partition(nums.begin(), nums.end()， nonzero);
	vector<double> zeros(iter, nums.end());
	students.erase(iter, nums.end());

### replace()

	int myints[] = { 10, 20, 30, 30, 20, 10, 10, 20 };
	vector<int> myvector (myints, myints+8);            // 10 20 30 30 20 10 10 20
	replace (myvector.begin(), myvector.end(), 20, 99); // 10 99 30 30 99 10 10 9

### reverse()

	vector<int> myvector = { 1,2,3,4,5,6,7,8,9 };
	reverse(myvector.begin(), myvector.end());       // 9 8 7 6 5 4 3 2 1


### swap()  泛型

	int x=10, y=20;                         // x:10 y:20
  	swap(x,y);                              // x:20 y:10


### binary_search()

	vector<int> v = {1,2,3,4,5,6};
	sort (v.begin(), v.end());
	if (binary_search (v.begin(), v.end(), 3)) {}

### fill(b,e,t)
把由输入迭代器b和e界定的序列的值设为t。返回void类型

### lexicographical_compare(b,e,b2,e2)
由 [b,e) 界定的序列是否比 [b2,e2) 界定的序列小（这个小的意义 同字符串大小的比较）。通过 `<`运算符 比较。

### lexicographical_compare(b,e,b2,e2,p)
同上，不过是用 `函数p` 来进行比较

### max_element(b,e)
### min_element(b,e)

### replace\_copy()和reverse\_copy()

![](imgs/algorithm0.png)

### unique()

![](imgs/unique.png)

### TIPS
* sort 、 remove_if 、 partition 函数都不会改变容器的大小，缩短容器是交给容器的erase成员函数来完成的，<font color=red>体现了的是改变容器自身属性的行为应该由容器自己来负责</font>

# <numeric\>  算法相关， 数值运算
### accumulate()

	double average(const vector<double>& v) {
		return accumulate(v.begin(), v.end(), 0.0) / v.size();
	}
> 对给定区间求和，第三个参数指定了求和结果的开始，和的类型就是第三个参数的类型


# <iterator\>
### back_inserter()
用一个容器作为它的参数并产生一个迭代器，在生成的迭代器被用作一个目的地的时候，他会<font color=red>向容器末端添加数值</font>。  
例如，在back_inserter(ret)被用作目的地的时候，它会为ret添加一个元素。   

	copy(bottom.begin(), bottom.end(), back_inserterer(ret));  
> 会扩容， 复制和扩容被分离了，扩容由back_inserter()来处理

	copy(bottom.begin(), bottom.end(), ret.end());
> 这个运行时会出错，没有扩容操作了, <font color=red>越界</font>访问了。 但是通得过编译。

### 插入迭代器
>	<font color=red>`back_inserter`</font>：创建一个使用push\_back的迭代器,这个容器必须支持链表、向量以及字符串类型都会支持的push\_back操作  
>	<font color=red>`inserter`</font>：此函数接受第二个参数，这个参数必须是一个指向给定容器的迭代器。**元素将被插入到给定迭代器所表示的元素之前**。这个容器必须支持push\_front操作----链表会支持，但是字符串和向量不支持。   
>	<font color=red>`front_inserter`</font>：创建一个使用push\_front的迭代器（**元素总是插入到容器第一个元素之前**）  


# <cstdlib\>

rand()  
返回在域[0, RAND_MAX) 中的一个随机整数  

	记得在程序中用 srand((unsigned)time(NULL)) 初始化随机数种子，不然结果都一样

	自定义的nrand(n)函数，返回在域[0,n)中的一个随机的整数    

```
int nrand(int n) {
	if (n<0 || n>RAND_MAX)
		throw logic_error("Argument to nrand is out of range.");
	const int bucket_size = RAND_MAX / n;
	int r;
	do {
		r = rand() / bucket_size;
	} while (r >= n);

	return r;
}
```

> 把可利用的随机数分到**长度相等**的存储桶中，计算一个随机数并且返回它所在的编号

>* 试图通过 `rand()%n` 来返回 [0,n) 的随机数会<font color=red>失败</font>, 因为 rand() 返回的只是 <font color=red>伪随机数</font>，当商是小整数的时候，许多C++系统环境的伪随机数生成器所产生的余数并不是绝对随机的，   
>* 第二个原因如果n<font color=red>很大</font>，那么RAND_MAX就不会均匀地被n除尽。如：RAND_MAX取32767（所有实现中，至少都要达到这个值），n取20000， 那么15000（只有15000），而10000（可以是10000或30000）, 得到10000的概率是15000的两倍


>rand的内部实现是用线性同余法做的，他不是真的随机数，只不过是因为其周期特别长，所以有一定的范围里可看成是随机的，式子如下  
rand = rand*const\_1 + c\_var;   
srand函数就是给它的第一个rand值。

> 用"int x = rand() % 100;"来生成 0 到 100 之间的随机数这种方法是不或取的，
比较好的做法是： j=(int)(ｎ*rand()/(RAND_MAX+1.0))　产生一个0到ｎ之间的随机

# static
* 用在函数中的局部变量中作用是具有全局寿命，即函数中只初始化一次, static const 初始化后不能再改变
* 用在一个文件的全局变量上，表示仅当前文件可见

# typedef
简写，并且增强<font color=red>可读性</font>

	typedef vector<string> Rule;  //规则的类型
	typedef vector<Rule> Rule_collection;  //规则集合的类型
	typedef map<string, Rule_collection> Grammer;  //映射表的类型


# 容器
### empty()
判空用empty()，而不要用长度去判空，因为有的容器empty()效率比求长度要快很多。

# 异常
所有层次没有一个对异常进行catch的话程序会终止

# 定义一个代表函数的参数

	void write_analysis(double analysis(const vector<string>&)) {
		vector<string> strs = {" dsf", "dsaf"};
		cout << analysis(strs);
	}


# TIPS
* <font color=red>`算法` 、`容器` 、`迭代器`</font> ，三者配合，C++的思想
* 算法作用于容器的**元素** ---- 并不是作用于容器
* 把find()独立到算法库，而不是当作容器的成员，提高了C++的**灵活型**，同时降低了标准库的**复杂性**，体现的是更高级的设计。

# 注意
* 对于有限编译器，容器类型嵌套是两个 `>` 之间要有<font color=red>**空格**</font>, 如 `map<string, vector<int> >` 否则编译器无法识别
