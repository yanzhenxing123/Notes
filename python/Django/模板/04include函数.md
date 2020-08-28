include：

1.include(module, namespace=None):	

		+ module:子url的模块字符串
		+ namespace：实例命名空间，如果要指定实例命名空间，那么一定要先指定应用命名空间。也就是在子url.py的文件中添加app_name变量。

2.

```python
path("cms2/", include(("cms.urls","cms"), namespace="cms2")) 
```

include的第一个参数是子url的名称，第二个参数是应用命名空间，用一个元组进行包装。



3.

```python
path("movie/", include([
    path("", views.movie),
    path("list/",views.movie_list)
])),
```

