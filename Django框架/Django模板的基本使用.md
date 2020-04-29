## 模板的基本使用

1、模板是 HTML 页面      

2、创建模板目录：

​		templates 目录下创建对应项目的模板目录，/project/templates/myApp  

3、配置路径：		

```python
# 修改 settings.py 文件 TEMPLATES -> 'DIRS': [os.path.join(BASE_DIR,'templates')],
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

4、定义模板文件:   

​		grades.html 、students.html  
​		1) 语法1：{{输出值，变量，对象.属性}}  
​		2) 语法2：{%执行代码%}

5、定义视图：	  

```python
def grades(request):
    # 从模板里取数据
    gradesList = Grades.objects.all()

    # 将数据传递给模板，模板在渲染页面，将渲染好的页面返回给浏览器
    return render(request,'myApp/grades.html',{"grades":gradesList})
```

6、配置 Url：  	

```python
url(r'^grades/$',views.grades()),url(r'^grades/(\d+)$',views.studentByGrade())
```

