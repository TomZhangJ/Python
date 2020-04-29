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

