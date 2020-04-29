# ***Django 开发流程***

------

## 1、创建数据库

```
create database kaishen;
```



## 2、创建工程

```
django-admin startproject project
```



## 3、创建项目

```
python manage.py startapp myApp
```



## 4、激活项目

```python
# 1) 修改 settings.py 文件 INSTALLED_APPS
    INSTALLED_APPS = [
        'django.contrib.admin',
        'django.contrib.auth',
        'django.contrib.contenttypes',
        'django.contrib.sessions',
        'django.contrib.messages',
        'django.contrib.staticfiles',
        'myApp'
    ]
```



## 5、配置数据库

```python
# 1) 修改 __init__.py
    import pymysql
    pymysql.install_as_MySQLdb()


# 2) 修改 settings.py 文件 DATABASES
    DATABASES = {
        'default': {
            'ENGINE': 'django.db.backends.mysql',
            'NAME': 'sunck',
            'USER': 'root',
            'PASSWORD': '123456',
            'HOST': 'localhost',
            'PORT': '3306',
        }
    }
```





## 6、创建模型类 

```python
	# 在项目目录中的模型类文件 /project/myApp/models.py 中增加数据表类
    class Grades(models.Model):
        gname       = models.CharField(max_length=20)
        gdate       = models.DateTimeField()
        ggirlnum    = models.IntegerField()
        gboynum     = models.IntegerField()
        isDelete    = models.BooleanField(default=False)
        def __str__(self):
            return self.gname
```



## 7、生成迁移文件

    python manage.py makemigrations



## 8、执行迁移

    python manage.py migrate



## 9、启动

    python manage.py runserver
    
    ## ip 可以省略，表示本机；port 默认8000；http://127.0.0.1:8000



## 10、配置站点



## 11、创建模板目录/项目模板目录

    /project/templates 和 /project/templates/myApp



## 12、配置模板路径

```python
	# 修改 settings.py 文件
    TEMPLATES = [
        {
            'BACKEND': 'django.template.backends.django.DjangoTemplates',
            'DIRS': [os.path.join(BASE_DIR,'templates')],
            'APP_DIRS': True,
            'OPTIONS': {
                'context_processors': [
                    'django.template.context_processors.debug',
                    'django.template.context_processors.request',
                    'django.contrib.auth.context_processors.auth',
                    'django.contrib.messages.context_processors.messages',
                ],
            },
        },
    ]
```



## 13、修改 /project/project/urls.py   

```python
    from django.conf.urls import url,include
    urlpatterns = [
        url(r'^admin/', admin.site.urls),
        url(r'^',include('myApp.urls'))
    ]
```



## 14、创建  /project/myApp/urls.py

```python
    from django.conf.urls import url
    from . import views

    urlpatterns = [
        url(r'^$',views.index()),
        url(r'^(\d+)\$',views.detail()),
        url(r'^grades/$',views.grades()),
        url(r'^students/$',views.students()),
        url(r'^grades/(\d+)$',views.studentByGrade())
    ]
```