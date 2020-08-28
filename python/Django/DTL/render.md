#### render 和 render_to_string

```python
from django.shortcuts import render
from django.template.loader import render_to_string
from django.http import HttpResponse

def index(request):
    # html = render_to_string("index.html")
    # return HttpResponse(html)

    # 二者等价
    return render(request, "index.html")
```







