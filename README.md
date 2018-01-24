# Python_Django_Project

Is Django MVC? Not a "true" MVC framework, the "C" is handled by the framework itself.

Django is MTV(Model-Template-View):
Model: Data access layer, 
Template: Presentation layer, 
View: Business logic layer - Accesses the model and displays the appropriate template.

The Concept Of Apps (Module): Each project/website has separate "apps", can have a single app.

Python Virtual Env: Recommended way to use Django. Create isolated environments with their own directories. Separates your Django project instances.

下载安装python: https://www.python.org/downloads/, 勾选Add Python 3.6 to PATH
MySQL安装XAMPP: https://www.apachefriends.org/index.html
以管理员身份打开命令窗口: pip install virtualenvwrapper-win
mkvirtualenv py1  #安装Python Virtual Env, 不同环境之间切换的命令: workon py1
pip install django
pip install mysqlclient
django-admin startproject djangoproject  #创建项目命令,djangoproject是项目名称
python manage.py runserver  #运行项目的命令

"You have 13 unapplied migration(s)."  #意思是表没有创建
django默认使用sqllite数据库.

localhost/phpmyadmin/  #打开mysql数据库的管理页面

settings.py设置使用mysql数据库:
DATABASES = {
	'default':{
		'ENGINE':'django.db.backends.mysql',
		'NAME':'djangoproject',
		'USER':'root',
		'PASSWORD':'123456',
		'HOST':'localost',
		'PORT':''
	}
}

创建默认表: python manage.py migrate

python manage.py runserver启动服务,打开页面localhost:8000/admin打开登录页面
创建登录用户: python manage.py createsuperuser --username=steve --email=steve@qq.com

创建app: python manage.py startapp posts 会生成posts文件夹和文件, settings.py文件INSTALLED_APPS里面添加'posts',主urls.py文件中添加:
from django.conf.urls import url,include
urlpatterns=[url(r'^posts/',include('posts.urls'))]
posts文件夹下创建urls.py:
urlpatterns = [
	url(r'^$',views.index,name='index')  #^表示开始,$表示结束,表示访问localhost/post/即是访问views.py下面的index方法.
]
views.py:
from django.shortcuts import render
from django.http import HttpResponse
def index(request):
	#return HttpResponse('Hello World')
	return render(request, 'posts/index.html',{'title':'Latest Posts'})  #创建posts/templates/posts/index.html文件 
	
materializecss.com/getting-started.html

在models.py中创建Posts类, 运行命令python manage.py makemigrations posts创建了文件0001_Initial.py, 运行命令 python manage.py migrate 创建表posts_posts.
在posts/admin.py文件中注册:
from .models import Posts
admin.site.register(Posts)

class="center-align"不起作用, 设置style="display:block". display默认为inline.

Bug: 在admin页面会看到Postss的link,要去掉多余的s,解决方案:
在models.py文件中class Posts下添加:
def __str__(self):
	return self.title
class Meta:
	verbose_name_plural = "Posts"

设置root url为posts页面: 
在主urls.py中添加 url(r'^$',include('posts.urls'))

# Post List<br/>
![image](https://github.com/SteveWeiChen/python_django_project/blob/master/posts/Post%20List.png)<br/>
# Post Details<br/>
![image](https://github.com/SteveWeiChen/python_django_project/blob/master/posts/post_details.png)<br/>
