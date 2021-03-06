#排序
//to be continue

将一组元素的任意序列重排成一个按照关键字有序的序列

##目录
1. [插入排序](#insert)  
&nbsp;&nbsp;&nbsp;&nbsp;1.1 [直接插入排序](#simple_insert)
&nbsp;&nbsp;&nbsp;&nbsp;1.2 [折半插入排序](#bi_insert)
&nbsp;&nbsp;&nbsp;&nbsp;1.2 [希尔排序](#shell_insert)
2. [交换排序](#exchange)  
&nbsp;&nbsp;&nbsp;&nbsp;2.1 [冒泡排序](#bubble)
&nbsp;&nbsp;&nbsp;&nbsp;2.2 [快速排序](#quick)
3. [选择排序](#select)  
&nbsp;&nbsp;&nbsp;&nbsp;3.1 [简单选择排序](#simple_select)
&nbsp;&nbsp;&nbsp;&nbsp;3.2 [堆排序](#heap)
4. [归并排序](#merge)
5. [基数排序](#radix)
6. [性能对比](#compare)

----
<a id='insert'> </a>
##插入排序
每一趟将一个待排序的记录将关键字大小插入到前面已经排好序的子序列中，直到全部记录插入完成

<a id='simple_insert'> </a>
###直接插入
适用于顺序存储和链式存储的线性表，时间复杂度 $$$O(n^2)$$$ ,

	void InsertSort(SqList &L) 
	{      
		// 对顺序表L作直接插入排序。    
		int i,j;    
		for (i=2; i<=L.length; ++i)    
			if (L.r[i].key < L.r[i-1].key) {    
				// "<"时，需将L.r[i]插入有序子表    
				L.r[0] = L.r[i];                 // 复制为哨兵    
				for (j=i-1; L.r[0].key < L.r[j].key; --j)   
					L.r[j+1] = L.r[j];             // 记录后移    
				L.r[j+1] = L.r[0];               // 插入到正确位置    
			}    	} // InsertSort 


<a id='bi_insert'> </a>
###折半插入
当 n 较大时, 总排序码比较次数比直接插入排序的最坏情况要好得多, 但比其最好情况要差
	
	void BInsertSort(SqList &L)
	{     
		// 对顺序表L作折半插入排序。    
		int i,j,high,low,m;    
		for (i=2; i<=L.length; ++i) {    
			L.r[0] = L.r[i];       // 将L.r[i]暂存到L.r[0]    
			low = 1;    high = i-1;    
			while (low <= high) {    // 在r[low..high]中折半查找有序插入的位置 
				m = (low+high)/2;                            // 折半    
				if (L.r[0].key < L.r[m].key) 
					high = m-1;              // 插入点在低半区    
				else   
					low = m+1;              // 插入点在高半区  
			}    
			
			for (j=i-1; j>=high+1; --j) 
					L.r[j+1] = L.r[j];  // 记录后移   
			L.r[high+1] = L.r[0];         // 插入  
		}    
	} // BInsertSort  

<a id='shell_insert'> </a>
###希尔排序
取一个小于n的步长d,距离为d的为一组做直接插入排序,然后步长递减
不稳定,适用于顺序表,空间复杂度$$$O(1)$$$.时间平均复杂度$$$O(n^{1.3})$$$

	//这段代码是间隔为dk的直接插入排序
	void ShellInsert(SqList &L, int dk) 
	{      
		// 对顺序表L作一趟希尔插入排序。本算法对算法10.1作了以下修改：    
		// 1. 前后记录位置的增量是dk，而不是1;  
		// 2. r[0]只是暂存单元，**不是哨兵**。当j<=0时，插入位置已找到。    
		int i,j;    
		for (i = dk+1; i <= L.length; ++i)    
			if (L.r[i].key < L.r[i-dk].key) { //需将L.r[i]插入有序增量子表  
				L.r[0] = L.r[i];                   // 暂存在L.r[0]    
				for (j=i-dk; j>0 && (L.r[0].key < L.r[j].key); j-=dk)   
					L.r[j+dk] = L.r[j];              // 记录后移，查找插入位置  
				L.r[j+dk] = L.r[0];                // 插入   
			}   
	} // ShellInsert  	void ShellSort(SqList &L, int dlta[], int t) {   
		// 按增量序列dlta[0..t-1]对顺序表L作希尔排序。    		for (int k=t-1; k>0; --k)    
			ShellInsert(L, dlta[k]);  // 一趟增量为dlta[k]的插入排序    	} // ShellSort    


----
<a id='exchange'> </a>
##交换排序
所谓交换,就是根据序列中两个元素的关键字比较结果来交换这两个元素在序列的位置,即比较后交换两个的值

<a id='bubble'> </a>
###冒泡排序
每一趟从前往后比较,小的放在前面,每一趟冒泡都将剩余最小的元素放在前面;时间复杂度为$$$O(n^2)$$$

	void swap(Elemtype &a,Elemtype &b)
	{
		Elemtype tmp;
		tmp = a;
		a = b;
		b = tmp;
	}
	
	void BubbleSort(SeqList R) 
	{    
		int i，j;    		Boolean exchange; //交换标志    		for(i = 1; i < n; i++){ //最多做n-1趟排序    
			exchange=FALSE; //本趟排序开始前，交换标志应为假   
			 			for(j=n-1;j>=i;j--) //对当前无序区R[i..n]自下向上扫描    				if(R[j+1].key < R[j].key){//交换记录    					swap(R[j+1].key,R[j].key);    					exchange=TRUE; //发生了交换，故将交换标志置为真    
			}  
			  
			if(!exchange) //本趟排序未发生交换，提前终止算法    				return;    		} //endfor(外循环)    	} //BubbleSort    


<a id=quick'> </a>
###快速排序
任取一个元素pivot作为基准,将表分为前后两个区,前面的比pivot小,后面的比pivot大,基于分治的思想,关键在于要平衡划分,可以选择low,high,(low+high)/2中间大小的值作为pivot

不稳定,时间复杂度为$$$nlog_2n$$$


	Elemtype mid(int a;int b)
	{
		if ( L.r(a).key < L.r(b).key)
	}
	
	int Partition(SqList &L, int low, int high) 
	{      		// 交换顺序表L中子序列L.r[low..high]的记录，使枢轴记录到位，        		// 并返回其所在位置，此时，在它之前（后）的记录均不大（小）于它                             		Elemtype pivotkey;                		pivotkey = L.r[low].key;     // 用子表的第一个记录作枢轴记录        		while (low < high) {           // 从表的两端交替地向中间扫描        			while (low<high && L.r[high].key >= pivotkey) 
				--high; 
			swap(L.r[low].key,L.r[high].key) //将比枢轴记录小的记录交换到低端        			while (low<high && L.r[low].key <= pivotkey) 
				++low;        			swap(L.r[low].key,L.r[high].key) // 将比枢轴记录大的记录交换到高端        		}        		return low;                  // 返回枢轴所在位置        	} // Partition        
	int Partition(SqList &L, int low, int high) 
	{       		// 交换顺序表L中子序列L.r[low..high]的记录，使枢轴记录到位，        
		// 并返回其所在位置，此时，在它之前（后）的记录均不大（小）于它        		Elemtype pivotkey;        		pivotkey = L.r[low].key;      // 枢轴记录关键字        		while (low<high) {            // 从表的两端交替地向中间扫描        			while (low<high && L.r[high].key>=pivotkey) 
				--high;        			L.r[low].key = L.r[high].key;  // 将比枢轴记录小的记录移到低端        			while (low<high && L.r[low].key<=pivotkey)
				++low;        
			L.r[high].key = L.r[low].key;      // 将比枢轴记录大的记录移到高端        		}        		L.r[low].key = pivotkey;            // 枢轴记录到位        
		return low;                   // 返回枢轴位置        	} // Partition        
	void QSort(SqList &L, int low, int high) 
	{          
		// 对顺序表L中的子序列L.r[low..high]进行快速排序        		int pivotloc;        		if (low < high) {                      // 长度大于1  
			pivotloc = Partition(L, low, high);  //将L.r[low.high]一分为二  
			QSort(L, low, pivotloc-1); //对低子表递归排序，pivotloc是枢轴位置  
			QSort(L, pivotloc+1, high);          // 对高子表递归排序        		}        	} // QSort    
	void QuickSort(SqList &L) 
	{    
		// 对顺序表L进行快速排序    
		QSort(L, 1, L.length);    	} // QuickSort   


----
<a id='select'> </a>
##选择排序
每一趟从后面n-i+1个待排序元素中选择最小的元素,作为第i个元素,进行n-1趟,最后一个元素不需要了

<a id='simple_selct'> </a>
###简单选择排序
选择排序即找出最小值的位置,然后将其放到最终的位置,
不稳定,时间复杂度$$$O(n^2)$$$,时间复杂度为O(1)

	void SelectSort(SqList &L) 
	{      
		// 对顺序表L作简单选择排序。    		int i,j,min;    		for (i=1; i < L.length; ++i) { // 选择第i小的记录，并交换到位   
			min = i; 			for(j = i+1; j < L.length; j++) // 在L.r[i..L.length]中选择key最小的记录  
				if (L.r[j] < L.r[min])
					min = j;			if (i!=min)                 // L.r[i]←→L.r[j];    与第i个记录交换    			swap(L.ri] , L.r[min])          		}    	} // SelectSort   


<a id='heap'> </a>
###堆排序
是一种树形选择排序法,在排序的过程中,看做一颗完全二叉树的顺序存储结构,分为最小堆和最大堆;
先建立初始堆,然后自下而上调整为最小(大)堆;然后交换堆顶和最后一个元素再重新调整堆为最小(大)堆
空间复杂度为O(1),时间复杂度为$$$nlog_2n$$$

	void HeapAdjust(HeapType &H, int s, int m) 
	{      		// 已知H.r[s..m]中记录的关键字除H.r[s].key之外均满足堆的定义，    		// 本函数调整H.r[s]的关键字，使H.r[s..m]成为一个大顶堆    
		// （对其中记录的关键字而言）    		int j;        
		H.r[0].key = H.r[s].key;    		for (j = 2*s; j <= m; j*=2) {   // 沿key较大的孩子结点向下筛选    			if (j < m && H.r[j].key < H.r[j+1].key) 
				++j;                      //j为key较大的记录的下标    			if (H.r[0].key >= H.r[j].key) 
				break;         // 应插入在位置s上   
			else{   				H.r[s].key = H.r[j].key;   
				s = j;
			}    		}    		H.r[s].key = H.r[0].key;  // 插入正确地位置    	} // HeapAdjust    
	void HeapSort(HeapType &H) 
	{      
		// 对顺序表H进行堆排序。    
		int i;    		for (i = H.length/2; i>0; --i)  // 把H.r[1..H.length]建成大顶堆    			HeapAdjust ( H, i, H.length );    		for (i=H.length; i>1; --i) {    			swap(H.r[1].key;H.r[i].key); // 将堆顶记录和当前未经排序子序列Hr[1..i]中 ,最后一个记录相互交换    			HeapAdjust(H, 1, i-1);  // 将H.r[1..i-1] 重新调整为大顶堆    
		}    	} // HeapSort    


----
<a id='merge'> </a>
##归并排序
将两个或两个以上的有序表合并成一个新的有序表,分治法
空间复杂度为O(n),时间复杂度为$$$nlog_2n$$$

	Elemtype *B = (Elemtype *)malloc ((n+1)*sizeof(Elemtype)) 
	void Merge (Elemtype A[], int low, int mid, int high) 
	{    		// 将有序的A[i..m]和A[m+1..n]归并为有序的A[i..n]    
		int i,j,k;    		for (k=low; k<=high; k++)
			B[k] = A[k];   		for (i = low,j = mid+1,k=i;i <= mid && j<=high;k++) {
			if(B[i] <= B[j])
				A[k] = B[i++];
			else
				A[k] = B[j++];		 }
		while(i <= mid )
			A[k++] = B[i++]; //复制未检查完的表
		while(j <= high )
			A[k++] = B[j++];	} // Merge    
	void MSort(Elemtype A[], int low, int high)
	{    
		// 将SR[s..t]归并排序为TR1[s..t]		if(low < high)    			int mid = (low + high)/2  // 将A[low..high]平分为A[low..mid]和A[mid+1..high]  
			MSort(A,low,m);    // 递归地将A[low..m]归并为有序 
			MSort(A,m+1,high);  //递归地将A[mid+1..high]归并为有序    			Merge(A,low,mid,high); // 归并到A[low..high]    		}    	} // MSort    
	void MergeSort(SqList &L) 
	{      
		// 对顺序表L作归并排序。    
		MSort(L.r, L.r, 1, L.length);    	} // MergeSort    

----
<a id='radix'> </a>
##基数排序
采用多关键字排序,最低位优先基数排序

	void Distribute(SLList &L, int i, ArrType &f, ArrType &e) 
	{      
		// 静态链表L的r域中记录已按(keys[0],...,keys[i-1])有序，    
		// 本算法按第i个关键字keys[i]建立RADIX个子表，    		// 使同一子表中记录的keys[i]相同。f[0..RADIX-1]和e[0..RADIX-1]    		// 分别指向各子表中第一个和最后一个记录。    		int j, p;    		for (j=0; j<RADIX; ++j) 
			f[j] = 0;     // 各子表初始化为空表    
		for (p=L.r[0].next; p; p=L.r[p].next) {    			j = L.r[p].keys[i]-'0';  //将记录中第i个关键字映射到[0..RADIX-1]，    		if (!f[j]) 
				f[j] = p;    			else 
				L.r[e[j]].next = p;    			e[j] = p;                // 将p所指的结点插入第j个子表中    		}    	} // Distribute    
	void Collect(SLList &L, int i, ArrType f, ArrType e) 
	{      
		// 本算法按keys[i]自小至大地将f[0..RADIX-1]所指各子表依次链接成    
		// 一个链表，e[0..RADIX-1]为各子表的尾指针    
		int j,t;    
		for (j=0; !f[j]; j++);  // 找第一个非空子表，succ为求后继函数: ++    		L.r[0].next = f[j];  // L.r[0].next指向第一个非空子表中第一个结点    		t = e[j];    
		while (j<RADIX) {    			for (j=j+1; j<RADIX && !f[j]; j++);       // 找下一个非空子表    			if (j<RADIX) { // 链接两个非空子表 
				L.r[t].next = f[j];
				t = e[j]; 
			}    		}    		L.r[t].next = 0;   // t指向最后一个非空子表中的最后一个结点    	} // Collect    
	void RadixSort(SLList &L) 
	{      
		// L是采用静态链表表示的顺序表。    
		// 对L作基数排序，使得L成为按关键字自小到大的有序静态链表，    
		// L.r[0]为头结点。    		int i;    		ArrType f, e;    		for (i=1; i<L.recnum; ++i) 
			L.r[i-1].next = i;    		L.r[L.recnum].next = 0;     // 将L改造为静态链表    		for (i=0; i<L.keynum; ++i) {      			// 按最低位优先依次对各关键字进行分配和收集    			Distribute(L, i, f, e);    // 第i趟分配    			Collect(L, i, f, e);       // 第i趟收集    			print_SLList2(L, i);    		}    	} // RadixSort 
	
----
<a id='compare'> </a>
##性能比较	###复杂度名称 | 时间复杂度|  空间复杂度 |   稳定 |和初始状态关系
------- | --- | --------|---|----
简单插入 | $$$O(n^2)$$$  |O(1)|y|最好O(n)
折半插入|$$$O(n^2)$$$  |O(1)|y|最好O(n)
冒泡|$$$O(n^2)$$$  |O(1)|y
简单选择排序|$$$O(n^2)$$$  |O(1)|n
希尔||O(1)|n
快速排序|$$$O(nlog_2n)$$$  |$$$O(nlog_2n)$$$|n|最坏$$$O(n^2)$$$
堆排序|$$$O(nlog_2n)$$$  |O(1)|n
归并排序|$$$O(nlog_2n)$$$  |O(n)|y
基数排序|O(d(n+r))|O(r)|y

###具体问题
- 在序列基本有序时会导致快速排序变慢
- n比较小的时候，适合插入排序和选择排序；- 基本有序的时候，适合直接插入排序和冒泡排序；- n很大但是关键字的位数较少时，适合链式基数排序；- n很大的时候，适合快速排序、堆排序 、归并排序；- 无序的时候，适合快速排序；- 稳定的排序：冒泡排序、插入排序、归并排序、基数排序；


----
##外部排序(参考B树)

###二路归并排序
###多路归并排序
###置换-选择排序
###最佳归并树









