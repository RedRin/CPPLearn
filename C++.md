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