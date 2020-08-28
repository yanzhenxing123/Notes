#### 1.exact和iexact

exact：SELECT `article`.`id`, `article`.`title`, `article`.`content` FROM `article` WHERE `article`.`id` = 1

iexact：SELECT `article`.`id`, `article`.`title`, `article`.`content` FROM `article` WHERE `article`.`id` like 1

#### 2.contains和icontains

 contains：SELECT `article`.`id`, `article`.`title`, `article`.`content` FROM `article` WHERE `article`.`title` LIKE BINARY %中国高铁%

 icontains：SELECT `article`.`id`, `article`.`title`, `article`.`content` FROM `article` WHERE `article`.`title` LIKE %中国高铁%

#### 3.in

可以直接指定某个字段是否在某个集合中:

代码：`result = Article.objects.filter(title__contains="高铁")` 

也可以通过其他的表的字段来判断是否在某个集合中

代码：`categories = Category.objects.filter(article__id__in=[1,2,3])`

如果判断相关联的表的字段，用双下划线进行访问  ："__"

in：SELECT `category`.`id`, `category`.`name` FROM `category` INNER JOIN `article` ON (`category`.`id` = `article`.`category_id`) WHERE `article`.`id` IN (SELECT U0.`id` FROM `article` U0 WHERE U0.`title` LIKE BINARY %hello%)

in不仅仅可以指定元组列表，还可以为“QuerySet”

```python
articles = Article.objects.filter(title__contains="hello")
categories = Category.objects.filter(article__in=articles)
for category in categories:
    print(category)
```

#### 4.比较的查询条件

（1）gt 大于(greater than): SELECT `article`.`id`, `article`.`title`, `article`.`content`, `article`.`category_id` FROM `article` WHERE `article`.`id` > 2

（2）gte 大于等于(greater than or equal): SELECT `article`.`id`, `article`.`title`, `article`.`content`, `article`.`category_id` FROM `article` WHERE `article`.`id` >= 2

（3）lt 大于等于(greater than or equal): SELECT `article`.`id`, `article`.`title`, `article`.`content`, `article`.`category_id` FROM `article` WHERE `article`.`id` < 2

（4）lte 大于等于(greater than or equal): SELECT `article`.`id`, `article`.`title`, `article`.`content`, `article`.`category_id` FROM `article` WHERE `article`.`id` <= 2

#### 5.start with、istartwith和endwith、iendwith

开始与结束



#### 6.range

遍历一个列表，然后满足条件的返回，用的是between...and...

SELECT `article`.`id`, `article`.`title`, `article`.`content`, `article`.`category_id`, `article`.`create_time` FROM `article` WHERE `article`.`create_time` BETWEEN 2020-04-07 09:00:00 AND 2020-04-07 09:15:00

#### 7.date

SELECT `article`.`id`, `article`.`title`, `article`.`content`, `article`.`category_id`, `article`.`create_time` FROM `article` WHERE DATE(CONVERT_TZ(`article`.`create_time`, 'UTC', 'Asia/Shanghai')) = 2020-04-07