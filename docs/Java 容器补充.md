## RandomAccess 接口使用

[RandomAccess 接口使用](<https://www.jianshu.com/p/89aaaee1077e>)

Random Access List(随机访问列表)如 `ArrayList` 要实现此接口，Sequence Access List(顺序访问列表)如 `LinkedList` 不要实现。
因为两者的高效遍历算法不同。

随机访问列表使用循环遍历，顺序访问列表使用迭代器遍历。

```java
if (list instance of RandomAccess) {        
  for(int m = 0; m < list.size(); m++){}    
}else{       
  Iterator iter = list.iterator();       
  while(iter.hasNext()){}   
}
```

