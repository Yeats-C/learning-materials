# Java遍历map中value值
* 1.通过Map.entrySet遍历key和value，在for-each循环中使用entries来遍历.推荐，尤其是容量大时。
![image](https://user-images.githubusercontent.com/64882640/123404902-e61ac400-d5db-11eb-99c9-4e681fdd1874.png)

* 2.通过Map.keySet遍历key，通过键找值value遍历（效率低）,普遍使用，二次取值。
![image](https://user-images.githubusercontent.com/64882640/123404959-f3d04980-d5db-11eb-87bb-8fc2b2b54a45.png)

* 3.如果只需要map中的键或者值，你可以通过Map.keySet或Map.values来实现遍历，而不是用entrySet。在for-each循环中遍历keys或values。
![image](https://user-images.githubusercontent.com/64882640/123405018-02b6fc00-d5dc-11eb-9dc8-3c8bc2efdcf4.png)

* 4.通过Map.entrySet使用iterator遍历key和value。
![image](https://user-images.githubusercontent.com/64882640/123405078-106c8180-d5dc-11eb-8805-b5b5ee8ab7d4.png)

* 关于JAVA的遍历知识补充：
```
1、list和set集合都实现了Iterable接口，所以他们的实现类可以使用迭代器遍历，map集合未实现该接口，若要使用迭代器循环遍历，需要借助set集合。

2、使用EntrySet 遍历，效率更高。
```
