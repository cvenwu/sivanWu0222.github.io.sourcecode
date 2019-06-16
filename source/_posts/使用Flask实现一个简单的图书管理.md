---
title: 使用Flask实现一个简单的图书管理
tags:
  - Python
  - Flask
  - flask
  - Web
share: true
categories:
  - Flask
reward: true
comment: true
top: 2
repo: sivanWu0222 | FlaskPython
date: 2019-05-17 10:00:56
description:
---

## 项目演示以及讲解项目开发流程

1. 配置连接数据库
2. 添加书和作者模型
3. 添加数据
4. 使用模板显示数据库查询的数据
5. 使用WTF显示变淡
6. 实现相关增删逻辑

## 配置数据库

1. 导入SQLAlchemy
2. 配置数据库
3. 关闭动态跟踪修改
4. 创建数据库对象
5. 使用终端创建数据库

```python
from flask_sqlalchemy import SQLAlchemy

# 数据库配置
app.config['SQLALCHEMY_DATABASE_URI'] = 'mysql://root:123456@localhost:3306/flask_books'
# 关闭自动跟踪修改
app.config['SQLALCHEMY_TRACK_MODIFICATIONS'] = False

# 创建数据库对象，并将数据库对应与对应的app绑定起来
db = SQLAlchemy(app)

```

### 定义书和作者模型

- 模型继承自db.Model
- __tablename__ 指定表名
- db.Column() 标明是一个字段
- db.relationship 定义关系引用 



<!-- more -->

### 添加数据

```python
    db.drop_all()
    db.create_all()

    # 生成数据
    au1 = Author(name='老王')
    au2 = Author(name='老惠')
    au3 = Author(name='老刘')
    # 把数据提交给用户会话
    db.session.add([au1, au2, au3])
    # 提交会话
    db.session.commit()

    bk1 = Book(name='老王回忆录', author_id=au1.id)
    bk2 = Book(name='我读书少，你别骗我', author_id=au1.id)
    bk3 = Book(name='如何才能让自己更骚', author_id=au2.id)
    bk4 = Book(name='怎样征服美丽少女', author_id=au3.id)
    bk5 = Book(name='如何征服英俊少男', author_id=au3.id)
    # 将数据提交给会话
    db.session.add([bk1, bk2, bk3. bk4, bk5])
    # 提交会话
    db.session.commit()
    print('1')
```

### 显示作者数据

1. 查询所有作者信息，将消息传递给模板
2. 模板按照格式，依次for循环作者和书籍即可 

> 这里用到了关系引用，通过作者获得书名，但实际上作者表中只有书的id，这里使用了关系引用来获取书名

```python
<hr>
<ul>
{% for author in authors %}
    <li>{{ author.name }}</li>
    <ul>
        {% for book in author.books %}
            <li>{{ book.name }}</li>
        {% endfor %}
    </ul>

{% endfor %}
</ul>
```

### 使用WTF显示表单

1. 自定义表单类,需要继承FlaskForm
2. 模板中进行显示
3. 注意：设置secret_key / 编码 / csrf_token

自定义表单类：

```python
# 自定义表单类
class AuthorForm(FlaskForm):
    author = StringField(label='作者', validators=[DataRequired()])
    books = StringField(label='书籍', validators=[DataRequired()])
    submit = SubmitField(label='添加')
```

视图函数中创建表单对象并进行传递

```python
@app.route('/')
def index():
    # 查询所有作者信息并传递给模板
    authors = Author.query.all()
    # 创建自定义表单类
    form = AuthorForm()
    return render_template('books.html', authors=authors, form=form)

```

在对应的Html文件中使用表单

> Tips: 记得编写csrf_token,如果有flash消息，记得显示flash

```html
<form action="">
    <center>{{form.csrf_token()}}</center>
    <center>{{form.author.label}} {{ form.author }}</center><br>
    <center>{{form.books.label}} {{ form.books }}</center><br>
    <center>{{ form.submit }}</center><br>
    {# 显示消息闪现的内容 #}
    {% for message in get_flashed_messages() %}
        {{ message }}
    {% endfor %}
</form>
```

### 实现相关的增删逻辑

> 做表单验证记得添加对应的POST请求

> 采用默认的html文件中的表单需要记得修改method='post'，如果没有修改将会默认是get请求

#### 增加数据

1. 调用wtf验证函数用一行代码实现验证
2. 如果通过验证可以获取数据
3. 判断作者是否存在
4. 如果作者存在，判断书籍是否存在，如果没有重复书籍，则可以添加数据
5. 如果作者不存在，就添加作者和书籍
6. 验证不通过就提示错误

调用wtf验证函数用一行代码实现验证: form.validate_on_submit() 其中**form是自定义表单类的对象名**

```python
# 1. 调用wtf验证函数用一行代码实现验证
    if form.validate_on_submit():
        # 2. 如果通过验证可以获取数据
        author_name = form.author.data
        book_name = form.books.data
        print(author_name)
        print(book_name)
        # 3. 判断作者是否存在
        author = Author.query.filter_by(name=author_name).first()

        # 4. 如果作者存在
        if author:
            # 判断书籍是否存在，如果没有重复书籍，则可以添加数据
            book = Book.query.filter_by(name=book_name).first()
            if book:
                # 书籍存在
                flash('已存在同名书籍')
            else:
                # 书籍不存在
                try:
                    # 添加数据
                    new_book = Book(name=book_name, author_id=author.id)
                    db.session.add(new_book)
                    db.session.commit()
                except Exception as e:
                    print(e)
                    flash("添加书籍失败")
                    # 添加失败，数据库回滚
                    db.session.rollback()
        else:
            print('45')
            # 5. 如果作者不存在，就先添加作者再添加书籍
            try:
                # 添加数据
                new_author = Author(name=author_name)
                db.session.add(new_author)
                db.session.commit()

                new_book = Book(name=book_name, author_id=new_author.id)
                db.session.add(new_book)
                db.session.commit()
            except Exception as e:
                print(e)
                flash("添加作者和书籍失败")
                # 添加失败，数据库回滚
                db.session.rollback()
    else:
        # 6. 验证不通过就提示错误
        if request.method == 'post':
            flash("参数不全")
        print('2')

```

tips: **一定要记得在添加作者和书籍之后才查询作者并显示，否则新加的作者或书籍不会被显示**

#### 删除数据

#### 点击书籍旁边的删除

>  url_for redirect 控制代码块 与 for else 的使用

> 根据书籍的id删除书籍

流程：点击删除按钮 --> 将需要发送书籍的ID传入给删除书籍的路由--> 路由需要接收参数

> from flask import Flask, render_template, flash, request, redirect, url_for

>  **一般情况下redirect 与 url_for 是结合使用的**
> redirect: 就是用来重定向的

> url_for() 需要传入视图函数名，会返回该视图函数对应的路由地址

```python
>
> return redirect({{ url_for('index') }})
> 等同于return redirect('/')
```


```
> url_for() 除了第1个传入视图函数名，后面还可以在对应的视图界面中传入视图函数所需要的参数，参数名便是视图函数中的动态参数名
> <a href="{{ url_for('delete_book', book_id=book.id) }}">删除</a>
```

对应的视图函数：

```python
@app.route('/delete_book/<int:book_id>')
def delete_book(book_id):
    # 查询数据库对应id书籍是否存在，如果有就删除，没有就提示错误
    book = Book.query.get(book_id)
    if book:
        try:
            db.session.delete(book)
            db.session.commit()
        except Exception as e:
            print(e)
            flash('删除书籍出错')
            db.session.rollback()
    else:
        flash('书籍不存在')
# 如何返回当前网址 --> 重定向
    # url_for 需要传入视图函数名，会返回该视图函数对应的路由地址
    return redirect({{ url_for('index') }})
    # 等同于return redirect('/')
```


删除书籍对应的视图界面：

```html
{% for book in author.books %}
    {# 如果有书将会执行下面这一行代码 #}
    <li>{{ book.name }} <a href="{{ url_for('delete_book', book_id=book.id) }}">删除</a></li>
{% else %}
    {# 如果没有书将会执行下面这一行代码 #}
    <li>无</li>
{% endfor %}
```

删除书籍对应的功能函数：

```python
@app.route('/delete_book/<int:book_id>')
def delete_book(book_id):
    # 查询数据库对应id书籍是否存在，如果有就删除，没有就提示错误
    book = Book.query.get(book_id)
    if book:
        try:
            db.session.delete(book)
            db.session.commit()
        except Exception as e:
            print(e)
            flash('删除书籍出错')
            db.session.rollback()
    else:
        flash('书籍不存在')

    # 如何返回当前网址 --> 重定向
    # url_for 需要传入视图函数名，会返回该视图函数对应的路由地址
    return redirect(url_for('index'))
    # 等同于return redirect('/')

```

##### 点击作者旁边的删除

首先判断作者是否存在，如果存在，先删除作者名下对应的书籍然后再删除书籍

```python
@app.route('/delete_author/<int:author_id>')
def delete_author(author_id):
    # 1 查询是否有该id对应的作者
    author = Author.query.get(author_id)
    if author:
        # 先删除作者名下的书籍
        try:
            # 首先对书查询之后直接删除
            Book.query.filter_by(author_id=author.id).delete()

            # 删除作者
            db.session.delete(author)
            db.session.commit()
        except Exception as e:
            print(e)
            flash('删除作者出错')
            db.session.rollback()
    else:
        flash('作者不存在')

    return redirect(url_for('index'))

```



### 总结

做项目的时候，最好先从数据库入手，做模型的一个创建，表名，字段的设置，然后在模板中显示数据

注意csrf_token的问题，secret_key的问题，中文编码的问题

```python
# 中文编码问题的解决
import sys
reload(sys)
sys.setdefaultencoding('utf-8')
```