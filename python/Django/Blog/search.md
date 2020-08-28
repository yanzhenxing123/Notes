# search

## 注意：

在search之后，可以进行排序处理，最新或者最热

原因：它的流程：

+ 输入关键字之后，进入view视图函数中，默认的排序时最新。view处理完之后，context上下文之中的内容是{search，articles，order}，分别为：关键字、文章QuerySet对象、和排序方式返回模板template

+ 在此页面，点击最热，order被修改为total_views，即浏览量，排序修改了

+ ```html
  <a href="{% url 'article:article_list' %}?order=total_views&search={{ search }}">
      最热
  </a>
  ```

总结：无论怎样时刻记着 MVT模型，即Model View Template。