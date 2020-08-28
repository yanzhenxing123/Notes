#### 模型.objects:

这个对象是`django.db.models.Manager` 类的对象，这个类是一个空壳类，它上面的所有方法都是从`QuerySet` 这个类上面拷贝过来的。所以只需知道如何使用`QuerySet`即可。

#### filter/exclude/annotate:过滤/排除满足条件/给模型添加新的字段



#### values:

我们在查找数据时，并不是想把所有数据全部提取出来，我们有可能想要其中的几个字段，这时候就可以使用`values`

```python
books = Book.objects.values("id", "name")
```

返回的还是一个QuerySet但是不是一个模型，其中是包含是字典

`<QuerySet [{'id': 1, 'name': '三国演义'}, {'id': 2, 'name': '水浒传'}, {'id': 3, 'name': '西游记'}, {'id': 4, 'name': '红楼梦'}]>`

与F表达式进行连用：

```python
books = Book.objects.values("id", "name", author_name = F("author__name"))  # 自定义的名字不能与模型上的本身Field有冲突，所以不能命名为author
```

在values中也可以使用聚合函数形成一个新的字段，相当于annotate

```python
books = Book.objects.values("id", "name", order_num = Count("bookorder"))
print(books.query)  # SELECT `book`.`id`, `book`.`name`, COUNT(`book_order`.`id`) AS `order_num` FROM `book` LEFT OUTER JOIN `book_order` ON (`book`.`id` = `book_order`.`book_id`) GROUP BY `book`.`id` ORDER BY NULL
```

如果调用values方法的时候没有指定参数，那么会获取这个模型上的所有字段以及对应的值形成的字典。

```python
books = Book.objects.values()  # {'id': 1, 'name': '三国演义', 'pages': 987, 'price': 98.0, 'rating': 4.8, 'author_id': 3, 'publisher_id': 1}
```

#### valueslist

返回的是QuerySet，但其中装的是元组：

`<QuerySet [(1, '三国演义', 987, 98.0, 4.8, 3, 1), (2, '水浒传', 967, 97.0, 4.83, 4, 1), (3, '西游记', 1004, 95.0, 4.85, 2, 2), (4, '红楼梦', 1007, 99.0, 4.9, 1, 2)]>`

```python
books = Book.objects.values_list("name", flat=True)
# 只指定一个字段，设置flat=True的意思就是设置扁平化，那么返回的的值就是一个字符串：
# 以前：<QuerySet [('三国演义',), ('水浒传',), ('西游记',), ('红楼梦',)]>
# 现在：<QuerySet ['三国演义', '水浒传', '西游记', '红楼梦']>
```

但是flat只能用在指定的字段只有一个的情况下



#### all方法：

简单的返回一个QuerySet对象，这个QuerySet没有经过任何的修改。



#### select_related:

```python
books = Book.objects.select_related("author", "publisher")
for book in books:
    print(book.author.name)  # all()这种操作非常消耗性能
print(connection.queries)  # 返回[{'sql': 'SELECT @@SQL_AUTO_IS_NULL', 'time': '0.000'}, {'sql': 'SET SESSION TRANSACTION ISOLATION LEVEL READ COMMITTED', 'time': '0.000'}, {'sql': 'SELECT `book`.`id`, `book`.`name`, `book`.`pages`, `book`.`price`, `book`.`rating`, `book`.`author_id`, `book`.`publisher_id`, `author`.`id`, `author`.`name`, `author`.`age`, `author`.`email`, `publisher`.`id`, `publisher`.`name` FROM `book` INNER JOIN `author` ON (`book`.`author_id` = `author`.`id`) INNER JOIN `publisher` ON (`book`.`publisher_id` = `publisher`.`id`)', 'time': '0.000'}]
```

注意事项：只能查找本身具有外键的选项



#### prefetch_related:

这个方法类似与select_related方法，也是在进行查找语句的时候将指定条件的数据查找出来，但是这个方法可以解决多对多、多对一的的情况，这个方法会产生两个查询语句。所以，如果在这个方法中查询使用外键关联的模型的时候，也会产生两个查询语句，因此如果查询的是外键关联的模型的时候，建议使用`select_related`这个方法

在查询多对多或者多对一的关联对象的时候，示例：要查询图书中所有的订单：

```python
    # 自己是别人的外键生成的一个属性，是自己的属性，bookorder_set，即“BookOrder”小写+“_set”
    books = Book.objects.prefetch_related("bookorder_set")
    for book in books:
        print("=" * 30)
        print(book.name)
        orders = book.bookorder_set.all()
        for order in orders:
            print(order.id)
```

需要注意的是在使用`prefetch_related`查找出来的`bookorder_set`对象，不要再使用其它的方法，比如`filter`，因为这样，会产生更多的SQL语句，只需使用`all()`方法即可。但是你要查找出满足条件对象的话，可以使用`Prefetch`进行先指定查找条件，再进行`prefetch_related`查找，示例：查找出book订单中卖的价格大于等于90的book_id:

```python
    from django.db.models import Prefetch
    
    # 这样加条件就可以实现有条件的查找，并且实现过滤等其他操作
    prefetch = Prefetch("bookorder_set", queryset=BookOrder.objects.filter(price__gte=90))
    books = Book.objects.prefetch_related(prefetch)
    for book in books:
        print("=" * 30)
        print(book.name)
        orders = book.bookorder_set.all()  # 只能调用all方法，因为调用其他方法的时候前面所做的功亏一篑
        for order in orders:
            print(order.id)
```



#### defer 和 only

这两个的作用是相反的：

`defer`的作用是将所传的参数进行过滤，即过滤之后返回的值中就没有这个参，你想访问也可以，只是还得用SQL语句：

```python
# 将"book"这一字段过滤掉
books = Book.objects.defer("name")
```

`only`的作用和defer是相反的，它的的作用就像它的名字，即将一个字段提取出来，但是这个主键的话可以传也可以不传，就可以获取到，访问其他字段的时候还是需要SQL语句。

```python
books = Book.objects.only("name", "price")
# {'sql': 'SELECT `book`.`id`, `book`.`name`, `book`.`price` FROM `book`', 'time': '0.000'}]
```

二者与values这一方法还是有区别的，values返回的是一个QuerySet中装的字典，但这个返回是一个模型。



#### get：

返回的不是获取满足条件的对象，而不是QuerySet对象。

而且get比较“娇气“，因为只能返回一个满足条件的值，如果有多个记录都满足这一个条件，它就会报错

```python
book = Book.objects.get(pk=1)
print(book)
print(connection.queries)

# 返回的值：
# Book object (1)
'''[{'sql': 'SELECT @@SQL_AUTO_IS_NULL', 'time': '0.000'}, {'sql': 'SET SESSION TRANSACTION ISOLATION LEVEL READ COMMITTED', 'time': '0.000'}, {'sql': 'SELECT `book`.`id`, `book`.`name`, `book`.`pages`, `book`.`price`, `book`.`rating`, `book`.`author_id`, `book`.`publisher_id` FROM `book` WHERE `book`.`id` = 1 LIMIT 21', 'time': '0.000'}]'''
```



#### create:

创建一条数据，并保存到数据库中。

如果不用create的话，那么就是：

```python
publisher = Publisher(name="知了出版社")
publisher.save()

# SQL语句：[{'sql': 'SELECT @@SQL_AUTO_IS_NULL', 'time': '0.000'}, {'sql': 'SET SESSION TRANSACTION ISOLATION LEVEL READ COMMITTED', 'time': '0.000'}, {'sql': "INSERT INTO `publisher` (`name`) VALUES ('知了出版社')", 'time': '0.000'}]
```

用create方法：

```python
publisher = Publisher.objects.create(name="知了课堂")
print(publisher)
print(connection.queries)
# Publisher object (8)
[{'sql': 'SELECT @@SQL_AUTO_IS_NULL', 'time': '0.000'}, {'sql': 'SET SESSION TRANSACTION ISOLATION LEVEL READ COMMITTED', 'time': '0.000'}, {'sql': "INSERT INTO `publisher` (`name`) VALUES ('知了课堂')", 'time': '0.000'}]
```

以上两个方法就是等价的



#### get_or_create:

```python
# 没有得到结过就创建
result = Publisher.objects.get_or_create(name="知了abc出版社")
print(result)

# 返回的result是一个元组，里面装的是一个字段对象(<Publisher: Publisher object (9)>, False),True或者False代表的就是有没有创建新的一个字段
```



#### bulk_create:

这个和create十分相似，只是这个创建的字段可以是多个：

```python
publisher = Publisher.objects.bulk_create([
    Publisher(name="清华出版社"),
    Publisher(name="水杯出版社")
])
```



#### count：

获取指定的字段中的记录个数：

```python
count = Book.objects.count()
print(count)
# 'sql': 'SET SESSION TRANSACTION ISOLATION LEVEL READ COMMITTED', 'time': '0.000'}, {'sql': 'SELECT COUNT(*) AS `__count` FROM `book`', 'time': '0.000'
```



#### exists：

这一个方法做判断是最高效的。

判断字段是否存在：

```python
result = Book.objects.filter(name="三国演义").exists()
print(result)
print(connection.queries)

# True
# 'sql': "SELECT (1) AS `a` FROM `book` WHERE `book`.`name` = '三国演义' LIMIT 1

```



#### distinct:

`distinct`在SQL语句中的作用：去除重复的数据，示例：

```python
# 需求获取book销售的价格大于80元的书并剔除重复的书
books = Book.objects.filter(bookorder__price__gte=80).distinct()
print(books)
for book in books:
    print(book)
print(connection.queries)
```

#### update：

一次性将所有的数据都更新

一次性可以将所有满足条件的数据都删除

#### delete：

删除某一条记录的时候，如果这个记录是别的表的外键，那么你删除了这个记录，与之相关联的也会被删除，因为你是用的`on_delete=models.CASCADE`, 所以删除之前必须搞清楚你的表之间的关系。

