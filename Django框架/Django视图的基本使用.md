## 视图的基本使用

```python
# 1、定义视图
    from django.http import HttpResponse
    def index(request):
        return HttpResponse("sunck is a good man!")

# 2、配置 Url
    # 1) 修改 project 下的 urls.py
        from django.conf.urls import url,include
        from django.contrib import admin

        urlpatterns = [
            url(r'^admin/', admin.site.urls),
            url(r'^',include('myApp.urls'))
        ]
        
    # 2) 在 myApp 下新建 urls.py
        from django.conf.urls import url
        from . import views

        urlpatterns = [
            url(r'^$',views.index()),
            url(r'^(\d+)\$',views.detail()),
        ]
```