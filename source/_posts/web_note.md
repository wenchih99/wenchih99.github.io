title: Python-Web应用程序
author: WENCHIH CHANG
author_id: wenchih99@github.com
date: 2020-12-02 10:56:46
tags: django
language:
archives:
categories: [python,python-web]
---
# 项目笔记:Python-Web应用程序
<!--more-->
## 一、Django入门:
>ll_env为环境名,事先安装好python3
- 建立虚拟环境:`pyhton -m venv ll_env`
- 激活虚拟环境:`ll_env\Scripts\activate.bat`
- 停止虚拟环境:`ll_env\Scripts\deactivate.bat`
- 安装Django:`pip install Django`
- 在Django下新建项目:`django-admin.py startproject learning_log .(.表示让新项目使用合适的目录结构)`
- 列目录:`dir`
- 创建数据库:`python manage.py migrate`
- 运行服务器:`python manage.py runserver 端口号(端口号默认为8000)`
- 创建应用程序:`python manage.py startapp learning_logs(appname)`
- models.py储存模型(用来管理数据),激活模型需要在settings.py的元组INSTALLED_APPS把模型目录(appname)加进去
- 修改完数据,则需要修改数据库:
    - 修改models.py
    - 执行命令:`python manage.py makemigrations learning_logs(形成迁移表)`
    - 执行命令:`python manage.py migrate(应用迁移)`

## 二、Django管理网站
- 创建超级用户:`pyton manage.py createsuperuser`
- 向网站注册模型:修改admin.py,将我们创建的并且想要注册的模型引入:
    - `from appname.models import 模型名(类名)`
    - `admin.site.register(模型名)`
    
## 三、Django shell
>交互式终端,易于向数据库查询特定信息
- 打开终端:`python manage.py shell `
- 退出终端:`ctrl Z`
- 查看用户id
    - `from django.contrib.auth.models import User`
    - `for user in User.objects.all():`
        - `print(user.username,user.id)`
- 重置数据库:`pyhton manage.py flush`

## 四、创建网页
>1. 定义URL、编写视图和编写模板
>2. 每个URL都被映射到特定的视图->视图函数获取并处理网页所需的数据->视图函数通常调用一个模板,后者生成浏览器能够理解的网页
- 映射URL:打开urls.py定义变量urlpatterns列表,包含项目应用程序的URL
- 每个urls文件最初引入URL函数和模块
- 定义可在管理网站请求所有URL:`url(r'^admin',admin.site.urls)`
- 定义应用程序请求的URL:`url(r'',include(('appname.urls','appname'),namespace='appname'))`
- 定义主页URL:`url(r'^$',view.index,name='index')`,映射到视图(r表示字符串,^表示字符串起始,$表示字符串结束)
- 编写视图views.py:
    ```
    def index(request):
        return render(原始请求对象request,网页模板'learning_logs/index.html')
    ```
- 建立放模板文件夹(appname文件夹->templates文件夹->appname文件夹->模板文件)
- 创建其他网页(实行子模版继承父模板)

## 五、用户账户
### 输入数据
- 用户提交信息的页面都是表单,创建表单的最简单方式是使用ModelForm
- 函数reverse() 根据指定的URL模型确定URL,导入:`from django.urls import reverse`
- GET请求:只是从服务器读取数据的页面
- POST请求:用户需要通过表单提交信息
### 创建用户账户
- 创建应用程序users:`python manage.py startapp users`
- 创建登陆网页(URL->视图-模板)
    - URL:
        ```
       from django.contrib.auth.views import LoginView
       urlpatterns = [
            url(r'^login/$',LoginView.as_view(),{'template_name':'users/login.html'},name='login'),
       ]
        ```
    - 视图:定义的login视图是默认视图,无需修改
    - 模板:创建模板目录时,一定要将login.html放在registration文件夹,否则会出现网页错误
      - 一个应用程序的模板可继承另一个应用程序的模板 `{% extends "learning_logs/base.html" %} `
### 数据关联用户
- 限制访问:装饰器(decorator)`@login_required`,装饰器是为了检查用户是否登录
    - 在视图函数前面加上装饰器,即可限制访问
    - 重定向登陆页面,需要在settings.py文件夹下加入代码:`LOGIN_URL = '/users/login/'`
- 将模型关联至用户
    - `from django.contrib.auth.models import User`
    - 在最高层模型中加入:`owner = models.ForeignKey(User)`
- 只显示用户自己的数据:`topics = Topic.objects.filter(owner=request.user).order_by('date_added')`,fliter函数用来获取合适的数据
- 保护用户自己的数据
    - 导入异常Http404: from django.http import Http404 
    - 获取当前id的条目:`topic = Topic.objects.get(id=topic_id)`
    - 判断与请求的id是否一样:topic.owner == request.user,否则raise Http404
- 将新主题关联到用户:先修改,后保存数据库
    ```
    new_topic = form.save(commit=False) 
    new_topic.owner = request.user 
    new_topic.save()
    ```

## 六、页面样式设置
- 在虚拟环境下载应用程序django-bootstrap3:pip install django-bootstrap3
- 只要有新的应用程序加入,要想应用,就得在settings.py的INSTALL_APPS下添加其代码
- 最后就是修改模板.html文件

## 七、git初使用

### Ⅰ 
- 下载:[https://npm.taobao.org/mirrors/git-for-windows/],傻瓜式安装
- 配置git,打开git bash
    - 输入用户名及电子邮箱(均可虚构)
    - `git config --global user.name "username"`
    - `git config --global user.mail "username@example.com"`
- 项目文件常出现__pycache__文件夹,里面有*.pyc类文件,可在项目中创建.gitignore文件,并添加__pycache__/内容,即可忽略
- git命令一般在cmd内使用,并在仓库所在目录的终端下执行
- git版本号:`git --version`

### Ⅱ 
- 初始化仓库 `git init`
- 检查状态 `git status`
- 将文件加入仓库 `git add .(.表示未被跟踪的所有文件)`
- 执行初次提交 `git commit -m "message(此次操作的名字)" (-m表示记录此次操作)`
- 查看提交历史 `git log (内容有操作ID信息;后面加--pretty=oneline表示简洁重要模式输出)`
- 再次提交 `git commit -am "message" (-a表示提交所有修改的文件)`
- 撤销修改 `git checkout .(恢复到之前最后一次提交的状态)`
- 检出以前的提交 `git checkout ID前六个字符(离开分支master,进入分离头指针状态)`
- 回复到以前的指定状态 `git reset --hard ID前六个字符`
- 删除仓库 `rmdir /s .git(windows系统)`

## 部署学习笔记
- 安装git和Hreoku Toolbelt
- 在manage.py所在的文件夹下创建:
    - 创建包含包列表的文件`requirements.txt:pip freeze > requirements.txt`
    - 指定Python版本,创建runtime.text,内容为:python-3.7.3
- 登录:`heroku:heroku login`
- 创建项目:`heroku create`
- 推送项目到服务器:`git push heroku master`
- 核实正确启动服务器进程:`heroku ps`
- 在Heroku上建立数据库
    - `heroku run python manage.py migrate`
- 连heroku服务器:`heroku run bash`
    - 创建超级用户:`python manage.py createsuperuser`
- 修改URL名称:`heroku apps:rename name`
- 修改settings.py:`DEBUG=false`,以防被攻击,并修改`ALLOWED_HOSTS=['localhost']`,只允许本地托管该项目
- 修改通用模板(自带模板),则必须在settings.py的模板块中修改`'DIRS'=[os.path.join(BASE_DIR,'模板地址')]`
- 使用方法`get_object_or_404()`来防止引发500错误
    - `from django.shortcuts import get_object_or_404`
    - `topic = get_object_or_404(Topic, id=topic_id)`
    - 如果请求的主题或条目不存在,就导致404错误(未修改之前是404错误)
- 设置SECRET_KEY来实现大量的安全协议
- 删除项目:`heroku apps:destroy --app appname`

## PS:访问
- http://localhost:8000/admin/ **访问管理网站**
- **Django模型字段参考**
- **shell如何查询数据的文档**
- **Django模板文档**
- http://getbootstrap.com/ **查看Bootstrap提供的模板**
- https://heroku.com/ **Heroku官网**