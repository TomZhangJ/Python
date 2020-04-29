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



## 进入python shell
```
python manage.py shell
```



## 引入包和生成数据表数据
```python
    from myApp.models import Grades,Students
    from django.utils import timezone
    from datetime import *

    Grades.objects.all()
    grade1 = Grades()
    grade1.gname = "python04"
    grade1.gdate = datetime(year=2017,month=7,day=17)
    grade1.ggirlnum = 3
    grade1.gboynum = 70
    grade1.save()
```



## 查看数据

```python
Grades.objects.all()
```



## 查看某条数据

```python
Grades.objects.get(pk=n)
```



## 修改数据

```python
grade1.gboynum = 50
grade1.save()
```



## 删除数据

```python
grade1.delete() ### 物理删除
```



## 关联对象

```python
grade1 = Grades.objects.get(pk=1)

stu = Students()
stu.sname = "xxx"
stu.sgender = False
stu.sage = 20
stu.scontend = "xxxxxx"
stu.sgrade = grade1
stu.save()
```



## 获得关联对象的集合

```python
grade1.students_set.all()
```



## 新增数据，并且关联对象

```python
stu3 = grade1.students_set.create(sname=u'曾志伟'，sgender=True,scontend=u'我是曾志伟'，sage=45)
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



## Admin 站点管理

1) 内容发布：负责添加、删除、修改内容

2) 配置 Admin 应用： 	

```python
# 修改 settings.py 文件 - INSTALLED_APPS 'django.contrib.admin'
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

3) 创建管理员用户：

```
python manage.py createsuperuser

# http://127.0.0.1:8000/admin/
```

4) 汉化 django 管理页面：

```
# 修改 settings.py 文件
# 原：
LANGUAGE_CODE = 'en-us'
TIME_ZONE = 'UTC'

# 修改后：
LANGUAGE_CODE = 'zh-Hans'
TIME_ZONE = 'Asia/Shanghai'
```

5) 管理数据表：

​	(1) 修改 admin.py 文件，导入对象包和注册表；

```python
	from .models import Grades,Students
	
	class GradesAdmin(admin.ModelAdmin):
		......
	admin.sites.register(Grades,GradesAdmin)
	
	class StudentsAdmin(admin.ModelAdmin):
		......
	admin.sites.register(Students,StudentsAdmin)
```

​	(2) 自定义管理页面，增加页面 class 、列表页属性和添加、修改页属性;

```python
## 列表页属性

# 显示字段
list_display = ['pk','gname','gdate','ggirlnum','gboynum','isDelete']

# 过滤字段
list_filter = ['gname']

# 查询字段
search_fields = ['gname']

# 分页
list_per_page = 5

## 添加、修改页属性
# 规定添加和修改页属性的先后顺序
fields = ['ggirlnum','gboynum','gname','gdate','isDelete']

# 给属性分组，fields 与 fieldsets 不能同时使用   
fieldsets = [
            ("num",{"fields":['ggirlnum','gboynum']}),
            ("base",{"fields":['gname','gdate','isDelete']})
        ]
```

​	(3) 关联对象

```python
# TabularInline 横向

class StudentsInfo(admin.TabularInline):
    model = Students
    extra = 2

# StackedInline 纵向

class StudentsInfo(admin.StackedInline):
```

​	(4) 设置页面列的名称

```python
gender.short_description = "性别"
```

​	(5) 布尔值显示

```python
 def gender(self):
     if self.sgender:
     	return "男"
     else:
    	 return "女"
```

​	(6) 执行动作的位置

```python
# 顶部的执行框取消
actions_on_top = False

# 底部的执行框显示
actions_on_bottom = True
```

6) 使用装饰器注册

​	在类上增加 @admin.register(Students)，删除 admin.sites.register(Students,StudentsAdmin)



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