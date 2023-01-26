# 数据结构（Python)

枚举法：一个一个的试



```python
# 开发时间 2023/1/16 14:10


"""
a+b+c=1000 且 a^2+b^2=c^2  求出abc的所有可能
"""
import time

start_time = time.time()
count = 0
"""
花费时间100s
"""
for a in range(1001):
    for b in range(1001):
        for c in range(1001):
            if a + b + c == 1000 and a * a + b * b == c * c:
                count += 1
                print(a, b, c)

end_time = time.time()
print(f"使用时间{end_time - start_time}")
print(count)

```

```py
"""
花费时间0.23s
"""
for a in range(1001):
    for b in range(1001):
        c = 1000 - a - b
        if a * a + b * b == c * c:
            count += 1
            print(a, b, c)
```

**时间复杂度**：可以理解为参与真正计算步骤的数

**时间复杂度的几条基本计算规则**
1.基本操作，即只有常数项，认为其时间复杂度为O(1)
2.顺序结构，时间复杂度按加法进行计算
3.循环结构，时间复杂度按乘法进行计算
4.分支结构，时间复杂度取最大值
5.判断一个算法的效率时，往往只需要关注操作数量的最高次项，其它次要项和常数项可以忽略                                                                    6.在没有特殊说明时，我们所分析的算法的时间复杂度都是指最坏时间复杂度

![image-20230116153543686](C:\Users\Ruoji\AppData\Roaming\Typora\typora-user-images\image-20230116153543686.png)



![image-20230116155831929](C:\Users\Ruoji\AppData\Roaming\Typora\typora-user-images\image-20230116155831929.png)

![image-20230116155848295](C:\Users\Ruoji\AppData\Roaming\Typora\typora-user-images\image-20230116155848295.png)



## 算法的概念

算法是计算机处理信息的本质，因为计算机程序本质上是一个算法来告诉计算机确切的步骤来执行一个指定的任务。一般地，当算法在处理信息时，会从输入设备或数据的存储地址读取数据，把结果写入输出设备或某个存储地址供以后再调用。
算法是独立存在的一种解决问题的方法和思想
对于算法而言，实现的语言并不重要，重要的是思想
算法可以有不同的语言描述实现版本(如C描述、C++描述、Python描述等)，我们现在是在用Python语言进行描述实现。

## 算法的五大特性

1.输入:算法具有0个或多个输入
2.输出:算法至少有1个或多个输出
3.有穷性:算法在有限的步骤之后会自动结束而不会无限循环，并且每一个步骤可以在可接受的时间内完成
4.确定性:算法中的每一步都有确定的含义，不会出现二义性
5.可行性:算法的每一步都是可行的，也就是说每一步都能够执行有限的次数完成 



![image-20230116164554840](C:\Users\Ruoji\AppData\Roaming\Typora\typora-user-images\image-20230116164554840.png)





## 顺序表

按顺序去存放数据



元素外置的顺序表    

1.按照内存来存，类似指针



## 单链表



```python
# 开发时间 2023/1/17 22:42


"""实现一个单链表类"""


class Node:
    """节点"""

    def __init__(self, elem):
        self.elem = elem  # 实现初始化数据
        self.next = None  # 初始化结点，类似指针的操作


class SingleListLink:
    """单链表"""

    def __init__(self, node=None):
        """初始画链表头"""
        self.__head = node

    def is_empty(self):
        """判断链表是否为空
        判断链表头的结点是否为 None 为真则返回
        """
        return self.__head is None

    def lenth(self):
        """计算链表长度"""
        cur = self.__head  # 定义当前游标位置
        # 从头遍历     考虑特殊情况：空链表的时候，cur == None，不进入循环，直接返回count
        count = 0  # 计数
        while cur is not None:
            count += 1
            cur = cur.next
        return count

    def travel(self):
        """遍历链表"""
        cur = self.__head
        # 考虑特殊情况，空链表时：cur == None 不进入循环
        while cur is not None:
            print(cur.elem, end=" ")    # 要先打印当前的元素，再移动 cur
            cur = cur.next
        print("")   # 打印换行

    def add(self, item):
        """在表链表头插入元素 item"""
        node = Node(item)      # 初始化一个结点
        node.next = self.__head     # 先将node的下一个指向链接链表头，防止将链表打乱
        self.__head = node      # 最后把结点和链表头连接上

    def append(self, item):
        """再链表尾部添加元素 item ，尾插法"""
        node = Node(item)
        cur = self.__head
        # 考虑特殊情况：空链表：cur.next不存在
        if self.is_empty():
            self.__head = node  # 直接将 self.__head 指向 node
        else:
            while cur.next is not None:     # 使用 cur.next 防止 cur 移动到None
                cur = cur.next
            # 退出循环后 连接链表头
            cur.next = node

    def insert(self, pos, item):
        """在指定位置插入元素 item"""
        # 需要两个游标来定位
        pre = self.__head
        node = Node(item)
        # 考虑特殊情况：pos小于0   在这不实现反向插入  直接调用 add()
        if pos <= 0:
            self.add(item)
        # 考虑特殊情况，超过了链表长度，选择直接在结尾添加元素
        elif pos > self.lenth():
            self.append(item)
        else:
            count = 0   # 用来和pos做判断，定位位置
            while count != (pos-1):
                count += 1
                pre = pre.next
            # 定位到位置以后插入结点 pre指向pos-1位置
            node.next = pre.next
            pre.next = node

    def remove(self, item):
        """删除链表中的元素"""
        cur = self.__head
        pre = None
        # 考虑特殊情况：空链表
        if self.is_empty():
            return
        else:
            while cur is not None:
                if cur.elem == item:
                    # 考虑特殊情况：判断该节点是否是头节点
                    if cur == self.__head:
                        self.__head = cur.next
                        break
                    else:
                        pre.next = cur.next
                        break
                else:
                    pre = cur
                    cur = cur.next

    def seach(self, item):
        """查找链表元素是否存在"""
        cur = self.__head
        while cur is not None:
            if cur.elem == item:
                return True
            else:
                cur = cur.next
        return False


if __name__ == '__main__':
    node2 = Node(100)
    ll = SingleListLink(node2)
    ll.add(2)
    ll.add(1)
    ll.append(90)
    print(ll.is_empty())
    ll.travel()
    ll.insert(0, 0)
    ll.insert(3, 10)
    ll.insert(9, 120)
    ll.travel()
    ll.remove(0)
    ll.travel()

```

## 双链表

```py
# 开发时间 2023/1/17 23:55
# 开发时间 2023/1/17 22:42


"""实现一个双向链表链表类"""

class Node:
    """节点"""

    def __init__(self, elem):
        self.elem = elem  # 实现初始化数据
        self.next = None  # 初始化结点，类似指针的操作    后继结点
        self.prev = None  # 初始化前驱结点


class DoubleLinkLiset:
    """单链表"""

    def __init__(self, node=None):
        """初始画链表头"""
        self.__head = node

    def is_empty(self):
        """判断链表是否为空
        判断链表头的结点是否为 None 为真则返回
        """
        return self.__head is None

    def lenth(self):
        """计算链表长度"""
        cur = self.__head  # 定义当前游标位置
        # 从头遍历     考虑特殊情况：空链表的时候，cur == None，不进入循环，直接返回count
        count = 0  # 计数
        while cur is not None:
            count += 1
            cur = cur.next
        return count

    def travel(self):
        """遍历链表"""
        cur = self.__head
        # 考虑特殊情况，空链表时：cur == None 不进入循环
        while cur is not None:
            print(cur.elem, end=" ")    # 要先打印当前的元素，再移动 cur
            cur = cur.next
        print("")   # 打印换行

    def add(self, item):
        """在表链表头插入元素 item"""
        node = Node(item)  # 初始化一个结点
        # 考虑特殊情况：空链表
        if self.is_empty():
            self.__head = node
            return
        else:
            # 将node的下一个指向链接链表头，防止将链表打乱
            node.next = self.__head
            self.__head = node  # 然后把结点和链表头连接上
            node.next.prev = node   # 最后把node下一个结点的前驱结点指向回node

    def append(self, item):
        """再链表尾部添加元素 item ，尾插法"""
        node = Node(item)
        cur = self.__head
        # 考虑特殊情况：空链表：cur.next不存在
        if self.is_empty():
            self.__head = node  # 直接将 self.__head 指向 node
        else:
            while cur.next is not None:  # 使用 cur.next 防止 cur 移动到None
                cur = cur.next
            # 退出循环后 连接链表头
            cur.next = node
            node.prev = cur

    def insert(self, pos, item):
        """在指定位置插入元素 item"""
        # 需要两个游标来定位
        cur = self.__head
        node = Node(item)
        # 考虑特殊情况：pos小于0   在这不实现反向插入  直接调用 add()
        if pos <= 0:
            self.add(item)
        # 考虑特殊情况，超过了链表长度，选择直接在结尾添加元素
        elif pos > self.lenth():
            self.append(item)
        else:
            count = 0  # 用来和pos做判断，定位位置
            while count < pos:
                count += 1
                cur = cur.next
            # 定位到位置以后插入结点 pre指向pos-1位置
            node.next = cur
            node.prev = cur.prev
            cur.prev.next = node
            cur.prev = node

    def remove(self, item):
        """删除链表中的元素"""
        cur = self.__head
        # 考虑特殊情况：空链表
        if self.is_empty():
            return
        else:
            while cur is not None:
                if cur.elem == item:
                    # 考虑特殊情况：判断该节点是否是头节点
                    if cur == self.__head:
                        # 头节点，但是不一定是单链表
                        self.__head = cur.next
                        if cur.next is not None:
                            # 判断链表是否只有一个结点
                            cur.next.prev = None
                        break
                    else:
                        cur.prev.next = cur.next
                        if cur.next:
                            cur.next.prev = cur.prev
                    break
                else:
                    cur = cur.next

    def seach(self, item):
        """查找链表元素是否存在"""
        cur = self.__head
        while cur is not None:
            if cur.elem == item:
                return True
            else:
                cur = cur.next
        return False


if __name__ == '__main__':
    node2 = Node(100)
    ll = DoubleLinkLiset()
    ll.travel()
    ll.add(3)
    ll.add(2)
    ll.add(1)
    ll.travel()
    ll.append(90)
    # print(ll.is_empty())
    ll.travel()
    ll.insert(0, 0)
    ll.insert(3, 10)
    ll.insert(9, 120)
    ll.travel()
    ll.remove(0)
    ll.travel()

```



# ![image-20230118154925837](C:\Users\Ruoji\AppData\Roaming\Typora\typora-user-images\image-20230118154925837.png) 





![image-20230118162030508](C:\Users\Ruoji\AppData\Roaming\Typora\typora-user-images\image-20230118162030508.png)



![image-20230118162204112](C:\Users\Ruoji\AppData\Roaming\Typora\typora-user-images\image-20230118162204112.png)





![image-20230118225841819](C:\Users\Ruoji\AppData\Roaming\Typora\typora-user-images\image-20230118225841819.png)



