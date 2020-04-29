## 进入python shell

```commad
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

