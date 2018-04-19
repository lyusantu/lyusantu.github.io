---
title: 一道类似于约瑟夫环的逻辑问题
date: 2018-04-19 17:23:49
---

今日在[V2EX](https://www.v2ex.com)上看到了一个求解决的问题，类似于约瑟夫环，对此还是略感兴趣，所以使用递归进行了问题的解答，在这里记录一下解答过程。

问题如下：

```
20 个人围成一圈，每个人背上都贴有一个编号，依次为 1、2、...、20

现在从编号为 1 的人开始，从 1 依次递增报数，凡是报的数是 3 的倍数人离开圈子，剩下的人继续往下报数

问最后留下的那个人的编号是多少？请编写程序解决此问题。
```

第一次解答如下：

```java
import java.util.ArrayList; 
import java.util.List; 

public class Test { 

public static void main(String[] args) { 
	List<Integer> list = new ArrayList<Integer>(); 
		for (int i = 0; i < 20; i++) { 
			list.add((i + 1)); 
		} 
		numberOff(list); 
		System.out.println(list.get(0)); 
	} 

	public static List<Integer> numberOff(List<Integer> list) { 
		for (int i = 0; i < list.size(); i++) { 
			list.set(i, list.get(i) + 1); 
			if (list.get(i) % 3 == 0) { 
				list.remove(i); 
				if (list.size() == 1) 
					break; 
				else 
				--i; 
			} 
		} 
	return list.size() > 1 ? numberOff(list) : list; 
	} 
} 

```

回答完问题后才发现问题问的是“最后留下的那个人的编号是多少？”，所以又更正了一下代码，第二次解答如下：

```
import java.util.ArrayList;
import java.util.List;

public class Test {

	public static void main(String[] args) {
		List<Integer> list = new ArrayList<Integer>();
		for (int i = 0; i < 20; i++) {
			list.add((i + 1));
		}
		numberOff(list);
		System.out.println("最后留下的人的编号为：" + numberOff(list));
	}

	public static Integer numberOff(List<Integer> list) {
		int start = 0;
		while (list.size() > 1) {
			for(int i = 0; i < list.size(); i++){
				if (++start % 3 == 0)
					list.remove(i);
				if(list.size() == 1)
					break;
				--i;
			}
		}
		return list.get(0);
	}
}
```
