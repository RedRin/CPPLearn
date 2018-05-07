#vector
###erase()
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

###insert()
	void insert ( iterator position, InputIterator first, InputIterator last );
	eg: ret.insert(ret.end(), bottom.begin(), bottom.end());

###reverse(n)
保留空间以保存n个元素，但不对这些元素进行初始化。这个操作不会改变容器的大小。它仅仅会影响向量为了响应对insert或push_back的重复调用而分配内存的频率

###resize(n)
给vector一个新长度，这个长度等于n。如果n比当前长度小，那么，在这个向量中位于位置n之后的元素会被删除掉。如果n比当前长度大，那新的元素（初始化为默认值）会被添加到向量中。

#list 
不能用标准库的sort函数来排序list,而要调用它自己的sort方法

	sort()
	sort(cmp) 
> 使用适用于list元素类型的 `<` 运算符来排列元素，或者使用判定cmp
> 来排列元素

例子：

	list<Student_info> students;
	students.sort(compare);

#<string\>

###getline
	istream& getline ( istream& is, string& str );
> 返回 ```is```

###构造string(iterator first, iterator last)

###wstring
<font color=red>宽字符版</font>的string
	
	wstring wstr = "你好 ";
	wcout << wstr;
> 配合着 `wcout` 来使用

#iterator
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


###const_iterator 
const指的是不能改变指向的容器中的值，但是可以改变指向

#<cctype\>
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

#<algorithm\>
<font color=red>**这些算法函数 经过了优化，效率很高**</font>
###copy()

	copy(bottom.begin(), bottom.end(), back_inserterer(ret));
	等价于
	ret.insert(ret.end(), bottom.begin(), bottom.end());

> copy是一个泛型算法的例子， back_inserter()是一个迭代器适配器(定义在<iterator\>）

###find_if()
	find_if ( InputIterator first, InputIterator last, Predicate pred );
> Returns an iterator to the first element in the range [first,last) for which applying pred to it, is true.   
> 这个 pred 是一个自定义的 bool 函数， 它的参数类型是指定的容器的元素的类型，如string要填char  
> <font color=red>不能与 const_iterator 搭配 </font>   
> 失败时返回 `last`
 
###equal()

	bool equal ( InputIterator1 first1, InputIterator1 last1,
               InputIterator2 first2 );
	例子： 回文的一种计算方法
	return equal(s.begin(), s.end(), s.rbegin());
> 头两个迭代器指示了第一个序列，第三个参数则是第二个序列的起点。equal<font color=red>假设</font>第二个序列的长度足以容纳第一个序列的长度，因此不需要结尾迭代器

###find()

	static const string url_ch = "~;/?:@=&$-_.+!*`(),'";
	char c='=';	
	find(url_ch.begin(), url_ch.end(), c)
> 从url_ch的开始到结尾查询字符c，找到了就返回指示它位置的迭代器，否则返回第二个参数url_ch.end()

###search()

	static const string sep = "://";
	iter i = search(i, e, sep.begin(), sep.end());
> 在前一对迭代器指定的范围内，查找第二对迭代器指定的一串数据，找到了就返回指向这串数据第一个字符的迭代器，否则返回 `e` 

###remove_copy()

	vector<double> nonzero;
	remove_copy(s.homework.begin(), s.homework.end(), back_inserter(nonzero), 0);
> remove_copy函数查找与一个特定值匹配的所有值并把这些值从容器中“删除”掉。在输入序列中所有不被“删除”的值将被复制到目的地。  
> 这里的删除不是指传进来的容器，传来的容器<font color=red>不受影响</font>

###remove\_copy\_if() 

	remove_copy_if(students.begin(), students.end(), back_inserter(fail), pgrade);   

	bool pgrade(const Student_info& s) { return grade(s)>=60;}
> 类似remove_copy 只不过，这里是把满足pgrade的去掉，再复制剩余的

###remove_if()
“删除”满足谓词的所有元素

	students.erase(remove_if(students.begin(),students.end(),fgrade), students.end());
	
	bool fgrade(const Student_info& s) {return grade(s)<60;}

> remove_if“删除”的机制是把不满足谓词函数的都复制到容器的<font color=red>开头位置</font>,  
> 返回指向最后一个不被“删除"的数据的<font color=red>后一个</font>位置的迭代器

![](imgs/remove_if1.png)

![](imgs/remove_if2.png)


###partition() 和 stable_partition()
以一个序列作为参数并重新排列序列的元素，以使满足谓词的元素排在那些不满足的元素之前。    

partition可能会破坏原先的顺序，而stable_partition会让各区域的元素的相互位置保持不变。     
如：对按字母排好序了的学生姓名向量操作时。

返回第二区域的第一个元素的位置（迭代器）

	vector<double>::iterator iter = stable_partition(nums.begin(), nums.end()， nonzero);
	vector<double> zeros(iter, nums.end());
	students.erase(iter, nums.end());
 

###TIPS
* sort 、 remove_if 、 partition 函数都不会改变容器的大小，缩短容器是交给容器的erase成员函数来完成的，<font color=red>体现了的是改变容器自身属性的行为应该由容器自己来负责</font> 

#<numeric\>  算法相关， 数值运算
###accumulate()

	double average(const vector<double>& v) {
		return accumulate(v.begin(), v.end(), 0.0) / v.size();
	}
> 对给定区间求和，第三个参数指定了求和结果的开始，和的类型就是第三个参数的类型

###transform()

	transform(students.begin(), students.end(), back_inserter(grades), grade);
> 前两个迭代器指定了范围，一个输入序列  
> 第三个迭代器指示了一个目的地，它将保存第四个参数函数的运行结果，要保证目的地有足够的空间，这里用插入迭代器就肯定够空间了  
> 第四个参数是函数指针，transform将这个函数应用到输入序列的每一
个元素中
#<iterator\>
###back_inserter()
用一个容器作为它的参数并产生一个迭代器，在生成的迭代器被用作一个目的地的时候，他会<font color=red>向容器末端添加数值</font>。  
例如，在back_inserter(ret)被用作目的地的时候，它会为ret添加一个元素。   
	
	copy(bottom.begin(), bottom.end(), back_inserterer(ret));  
> 会扩容， 复制和扩容被分离了，扩容由back_inserter()来处理

	copy(bottom.begin(), bottom.end(), ret.end());
> 这个运行时会出错，没有扩容操作了, <font color=red>越界</font>访问了。 但是通得过编译。

###插入迭代器
<font color=red>`back_inserter`</font>：创建一个使用push\_back的迭代器   
<font color=red>`inserter`</font>：此函数接受第二个参数，这个参数必须是一个指向给定容器的迭代器。**元素将被插入到给定迭代器所表示的元素之前**。   
<font color=red>`front_inserter`</font>：创建一个使用push_front的迭代器（**元素总是插入到容器第一个元素之前**）

#static
* 用在函数中的局部变量中作用是具有全局寿命，即函数中只初始化一次, static const 初始化后不能再改变
* 用在一个文件的全局变量上，表示仅当前文件可见

#容器
###empty()
判空用empty()，而不要用长度去判空，因为有的容器empty()效率比求长度要快很多。

#异常
所有层次没有一个对异常进行catch的话程序会终止

#定义一个代表函数的参数

	void write_analysis(double analysis(const vector<string>&)) {
		vector<string> strs = {" dsf", "dsaf"};
		cout << analysis(strs);
	}


#TIPS
* <font color=red>`算法` 、`容器` 、`迭代器`</font> ，三者配合，C++的思想
* 算法作用于容器的**元素** ---- 并不是作用于容器
