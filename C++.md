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

#iterator
> `*` 的优先级比 `.` 运算符低， `*ite.name` 会被解释为 `*(ite.name)`   
> 而与 `++` 和 `--` 相同优先级， `*ite++` 解释为 先取 `*ite` 然后对 <font color=red>`ite`</font> 加一

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