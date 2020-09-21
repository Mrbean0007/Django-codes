# Django Project

## Create a virtual environment

`python -m venv django_env .`

### Activate virtual environment

```cmd
cd django_env/Scripts/

activate.bat
```

### Or'

`F:\Python Code\Django codes\django_env>.\Scripts\activate.bat`

## Install django

`pip install django`

## Checking  of django version

`python -m  django --version`

## List of Command associated with django-admin

`django-admin`

``` django
output

    check
    compilemessages
    createcachetable
    dbshell
    diffsettings
    dumpdata
    flush
    inspectdb
    loaddata
    makemessages
    makemigrations
    migrate
    runserver
    sendtestemail
    shell
    showmigrations
    sqlflush
    sqlmigrate
    sqlsequencereset
    squashmigrations
    startapp
    startproject
    test
    testserver
```

## Create project  

`django-admin startproject filename(django_project)`

## Running server  

`python manage.py  runserver`

Or (Try this )

`django-admin runserver`

## Create a Application

`python manage.py startapp AppName(Blog)`

## In views file (In Blog Application a file known as views.py)

views.py
: A view function, or view for short, is simply a Python function that takes a *Web request and returns a Web response.*
This response can be the HTML contents of a Web page, or a redirect, or a 404 error, or an XML document, or an
image . . . or anything, really.

```python
from django.shortcuts import render

from django.http import HttpResponse

''' The home function is going to handle traffic from our home page of our blog
 the home function will take a request a argument we aren't going to use request variable just yet we need to add request in oder to run our function
 We will use a return which will show what we want the user to see when their send to this route
 So this were the logic goes for how we want to handle certain route'''

def home(request):
    return HttpResponse('<h1>Blog Home </h1>')

```

This is the logic for how we want to handle when a user goes to our blog homepage but we haven't actually mapped our URL pattern to this view function just yet.
So to do this we need to create a new module in our Blog directory called url.py.
In url.py we'll map the url that we want to correspond to each view function.

### Creating a file know as url.py in Blog directory

1. If you see django_project directory it has a url.py file.In which we will be using djand.urls and url patterns.

2. We will also going to use home views function in views.py within our urls so we will import views.py module within our url.Where . mean current directory.

3. How we will create path for our home page.

    - path('') : Since we want it to be a home page we will keep this as empty path otherwise if we want it to route toward blog page or admin page it we path('blog/') or ('admin/').

    - path(views.home) : This will handle logic at that home page route and we want it to be our home views from our views module.

    - path(name='blog-home'):This will assin a name to this path when we hit the url name.

```python
from django.urls import path
from . import views

urlpatterns = [
    path('', views.home,name='blog-home'),
]
```

This will still not work since we have to include in our main url.py (django_project) module which will tell our whole website which urls should send us to our blog app.

### Mapping our application urls.py in main urls.py

We already have one route as admin which get mapped to these admin.site.urls.We will also do same things but we will tell django which route should get mapped to our blog urls.

So we need to import a function from django.urls i.e include.

include
: It help us specify which route should goes to our blog url.

```python
"""django_project URL Configuration

The `urlpatterns` list routes URLs to views. For more information please see:
    https://docs.djangoproject.com/en/3.1/topics/http/urls/
Examples:
Function views
    1. Add an import:  from my_app import views
    2. Add a URL to urlpatterns:  path('', views.home, name='home')
Class-based views
    1. Add an import:  from other_app.views import Home
    2. Add a URL to urlpatterns:  path('', Home.as_view(), name='home')
Including another URLconf
    1. Import the include() function: from django.urls import include, path
    2. Add a URL to urlpatterns:  path('blog/', include('blog.urls'))
"""
from django.contrib import admin
from django.urls import path,include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('blog/', include('Blog.urls')),

]
```

So when we open our webpage browser and go toword /blog then it will map to our blog url.

Then in our blog url.py we have that empty path path('',mapon our home views (views.home),name='blog-home')

Now use command running server  

`python manage.py  runserver`

If we navigate toward [localhost:8000](http://localhost:8000/)

![alt Error Message](Documentation_Images\image_1.jpg)

The error message says that it try to match [localhost:8000](http://localhost:8000/) with routed link as defined in django.project.urls first admin/ second blog/ but it could not find it.

But if we navigate toward [localhost:8000/blog](http://localhost:8000/blog/)

1. It first looks in our project.url.py module(main urls.py).

2. It check for the patterns which is been navigated(blog/).
If anything written after blog/ that is blog/MyBlog/xyz
would get choped off by include function because in our main url.py it would match blog/ and other are send toward Application Blog urls.py.

3. If the pattern matches then it ask for where do i want to send the people who go this route where in our case is Blog.urls.

4. The include part chope off what ever part it matches up to that point and sends remaining strings to include url module for futher processing. So our case blog/ get chopped off there is nothing remaining after  it just send empty strings to blog url.

5. So now in Blog/urls.py it will check for the pattern with empty route and that pattern will be handle by that function views.home.

6. It navigate toward views file in which home function and it simply says that we have to return httpsrresponse.

## Creating a About page

In views.py

```python
from django.shortcuts import render

from django.http import HttpResponse

def home(request):
    return HttpResponse('<h1>Blog Home </h1>')


def about(request):
    return HttpResponse('<h1>Blog About</h1>')

```

In urls.py

```python
from django.urls import path
from . import views

urlpatterns = [
    path('', views.home, name='blog-home'),
    path('about/', views.about, name='blog-about'),
]
```

<!-- It will give a error because it not associated with blog -->

<!-- [localhost:8000/about](http://localhost:8000/about/) -->

We are done with routeing.If you were think that do we need to add in our  main or project urls.py modules answer is no because when some goes to our blog route  send to our blog urls and then our blog urls will handle the about route.

[localhost:8000/blog/about](http://localhost:8000/blog/about/)

### How About page will route

1.Project url.py  will check pattern which matchs, if it has admin/ or blog/.So it found blog/about/.

2.It will send to include funtion which will chop the blog/ which is matched and send remaing strings which is about/ which will send to blog.urls.

3.In blog urls.py its looking for about/ to match pattern as it find about/ it would go views.about through that which will return https request.

### Change url in one place access it anywhere for that application

For an example you have blog which is in development and you want to do some live testing but weren't ready to make it live yet.

So in project or main urls.py change blog/ to blog_dev/ or any string name yoy would still be able to acces Blog app like  about/

In urls.py(main)

```python
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('blog_dev/', include('Blog.urls')),
]
```

Example [localhost:8000/blog_dev/](http://localhost:8000/blog_dev/)

Example [localhost:8000/blog_dev/about](http://localhost:8000/blog_dev/about/)

### Default homepage on localhost:8000

Keep the path url for Blog.url empty empty

In urls.py(main)

```python
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('Blog.urls')),
]
```

### Should we put a trailing slash or not (blog/ or blog_dev/)

By default if it has trailing slash then  django will redirect them without a forward slash or without a trailing slash otherwise django will put trailing or forward slash to that route to redirect it.

## Templates

```python
def home(request):
    return HttpResponse('<!DOCTYPE html>...')
```

As above example we can write an entire html code in HttpResponse.But problem is then we have to write entire html structure for each and every single views.So it will  get more and more complex and vode becomes big.So we are going create templates for each views.

**`Create a folder called templates in Blog directory`**

By default django look for it subdirectory and each of our install Apps for templates

Since django is going to look into other location for additional templates so will create another subdirectory within our templates directory (in our Blog app) so we know that which specific application tempates is in which directory **`Create a folder called Blog as our application name says Blog inside the templates folder`**

How will it look like

`Blog(App) -> templates(folder) -> blog(folder) -> templates.html(files)`

Create about.html and home.html

home.html

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Home</title>
</head>

<body>
    <h1>Blog home!</h1>
</body>

</html>

```

about.html

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>About</title>
</head>

<body>
    <h1>About page</h1>
</body>

</html>


```

So how we have create our home template we have to add our blog application to our list of install App so django knows to look there for a templates directory.

## Adding App to our project settings.py

It is recommended that our App configration model to add in our install app list located at Application Blog directory as app.py

```python
from django.apps import AppConfig


class BlogConfig(AppConfig):
    name = 'Blog'
```

So basically BlogConfigs inherts from AppConfig class here

Open django_project/setting.py file

Add the path of this class(BlogConfig) inside INSTALLED_APPS list

The reason we add to the path is so that django correctly find the templates and also when we work with our database.

```python

INSTALLED_APPS = [
    'Blog.apps.BlogConfig',
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
]

```

### Now we will Render Our templates in views.py

We use a shortcut rather than calling the templates in views.py than rendering it.Intead of that we will use django.shortcuts which has render functions.

Remember always that *View always return HttpResponse or Exceptions*

The below code is from stortcut.py

```python
def render(request, template_name, context=None, content_type=None, status=None, using=None):
    """
    Return a HttpResponse whose content is filled with the result of calling
    django.template.loader.render_to_string() with the passed arguments.
    """
    content = loader.render_to_string(template_name, context, request, using=using)
    return HttpResponse(content, content_type, status)
```

```python
from django.shortcuts import render
from django.http import HttpResponse

def home(request):
    return render(request,'Blog/home.html')


def about(request):
    return HttpResponse('<h1>Blog About</h1>')

```

Uptill now we have created static information html but what if we want to add dynamic information like post, update and all kindsof stuff.

To display dyanic information we will create some dummy data in list and pass as a content as argument of render.

1.We create a variable called post which would be list.

2.This list is going to be a lists of dictionaries and each dictionaries information would be of one post.

```python
#(In views.py)
posts = [
    {'auther': 'Mr.Bean',
     'title': 'Blog Post 1',
     'content': 'Lorem ipsum dolor sit amet.',
     'date_posted': '09 September 2020'
     },

    {'auther': 'Luffy D Monkey',
     'title': 'Blog Post 2',
     'content': 'Lorem ipsum dolor sit amet consectetur adipisicing elit. Quae, quaerat! Ullam.',
     'date_posted': '10 September 2020'
     },
]
  
```

3.Assume that we have gotten this from a database and want to diplay on our blog home page.

4.We can pass this post into our template, By passing an argument(context) with our data and we'll *put other data in dictionary*.

1. Create dictionary called context.

2. Create a key called posts(example)

3. Assign the posts key with our values list of posts dictionary as that we have created above.

4. Now we can  pass our thrid argument context.By add context we can pass  our data into our templates and let us access within the templates.

    ```python
    #In views.py
    def home(request):
    context = {'posts': posts}
    return render(request, 'Blog/home.html',context)

    # Note we can even delare this  posts data  inside home function and pass that into context variable.But make sure will looping.
    '''context={'auther': 'Mr.Bean',
     'title': 'Blog Post 1',
     'content': 'Lorem ipsum dolor sit amet.',
     'date_posted': '09 September 2020'
     },'''

    ```

5. So now whatever key name that we use in this dictionary that we passed it will be accesable from within the templates and then will be equal to that values.So this 'posts'(key) variable there and will be equal to this posts data which is a list of dictionaries containing information of each host

6. Switch to home.html so we can see how to use this.

7. We need to loop through those posts in order to diplay themon our homepage.Since django use templating engine that django is vey similar to jinja2.So templating engine allows to write code  with our templates

8. Create for loop in templates.To create a for loop we need to open up a code block {% for post in posts%} This posts variable we are looping over there is key of the context dictionary that we pass in.Always end for in templates {% end for %}.How we can post information one at time.

9. To acces a variable in templates we use {{ post.title }}

    ```html
    <body>
        <h1>Blog home!</h1>

        {% for post in posts %}

        <h2>{{ post.title }}</h2>
        <p>By {{ post.auther }} on {{post.date_posted}}</p>
        {% endfor %}
    </body>
    ```

Summary:- We create a list of dictionary(posts(gobally)) and then we create a dictionary(context) inside home function where dictionary key('posts') was assign to variable post (list of dictionary) and then it was past to the templates using render method.Inside the templates we create a loop to iterate our post.

We can aslo write like this

```python

def about(request):
    context_1 ={'auther': 'Luffy D Monkey',
                 'title': 'Blog Post 2',
                 'content': 'Lorem ipsum dolor sit amet consectetur adipisicing elit. Quae, quaerat! Ullam.',
                 'date_posted': '10 September 2020'
                 }
    return render(request, 'Blog/about.html', context_1)

    # But in this case you won't be able to loop

```

```html
<body>
    <h1>About page!</h1>
    <p>This is my bio</p>

    <h2>Hello I am {{ auther }}</h2>
    <p> {{ title }}</p>
</body>
```

### You can not pass a list directly in context since django templates does not render it.It need to be in dictionary

```python

def about(request):
    context_1 =[{'auther': 'Luffy D Monkey',
                 'title': 'Blog Post 2',
                 'content': 'Lorem ipsum dolor sit amet consectetur adipisicing elit. Quae, quaerat! Ullam.',
                 'date_posted': '10 September 2020'
                 }]
    return render(request, 'Blog/about.html', context_1)
```

![alt Error on list ](Documentation_Images\image_4.jpg)

### Using if else statment for page title

```html
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    {% if title %}
    <title>Django Blog {{ title }}</title>
    {% else %}
    <title>Django Blog </title>
    {% endif %}
</head>
```

```python

def about(request):
    return render(request, 'Blog/about.html', {'title':'About'})
```

### Intersting notes and Another method

```python
def home(request):
    context = {'posts': posts}
    return render(request, 'Blog/home.html', context)

# If you are thinking the title in posts variable would work sorry it won't work.The reason is that since we are not loop in <head> tags and even if we loop the first title is going to diplay and even if we remove it title for first section the second post till won't come .
#

# But for below code it would work since it has only one dictionary
def about(request):
    context_1 = {'auther': 'Luffy D Monkey',
                 'title': 'Blog Post 2',
                 'content': 'Lorem ipsum dolor sit amet consectetur adipisicing elit. Quae, quaerat! Ullam.',
                 'date_posted': '10 September 2020'
                 }
    return render(request, 'Blog/about.html', context_1,)
```

 Till now we were repeat or similar code in home and about page which is not a DRY pratices.
 Also if we want to update a section like title of templates we need to update all location  For example if we want change our default title we would need to make changes in both home and about.So it better to have a everthing that repeated  in a single place.So there is only one place to make changes and our home templates and about templates that is unique to those places so to accomplished this we need use something called templates inhertance  

## Templates Inhertance

1.Create a file in Blog templates called base.html

In that we have copy about.html(entire) and delete the`<h1>About Page</h1>` which was unquie to that entier page.
<!-- Now we are left html share between about and home views -->

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    {% if title %}
    <title>Django Blog {{ title }}</title>
    {% else %}
    <title>Django Blog {{ title }}</title>
    {% endif %}

</head>
<body>
    <h1>About page!</h1>
</body>
</html>
 ```

2.Create a Block in base.html.(A Block is section that child templates can override.)
We have create block called *content section* `{% block content %} {% endblockt %}`
Now in our home and about templates we will inherit this base template and get all the information and then we'll just override the content section with stuff that is unquie to each page.

```html
<body>
    {% block content %} {% endblock %}
</body>
```

3.We want to use home.html template to use that base.html as the parnet so we can get ride of all the information that is in that base template.(Removing the repeated code which is in home.html)

So this for loop was only unquie in our home.html

```html
{% for post in posts %}
<h2>{{ post.title }}</h2>
<p>By {{ post.auther }} on {{post.date_posted}}</p>
<p>{{ post.content }}</p>
{% endfor %}
```

4.To use base template we have to use extends {% extends "Blog/base.html"%}

```html
{% extends "Blog/base.html"%}
{% for post in posts %}
<h2>{{ post.title }}</h2>
<p>By {{ post.auther }} on {{post.date_posted}}</p>
<p>{{ post.content }}</p>
{% endfor %}
```

5.Remeber that in pur base.html we had a block content and we want our for loop in our home  template to override that content section so to that we can wrap it in our content block

```html
{% extends "Blog/base.html"%}
{% block content %}
{% for post in posts %}
<h1>{{ post.title }}</h1>
<p>By {{ post.auther }} on {{post.date_posted}}</p>
<p>{{ post.content }}</p>
{% endfor %}
{% endblock content %}
```

Similarly for about.html

```html
{% extends "Blog/base.html"%}
{% block content %}
<h1>About Page</h1>
{% endblock content %}
```

## Adding Bootstrap in base.html

```html

<!DOCTYPE html>
<html lang="en">

<head>
    <!-- Required meta tags -->
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">

    <!-- Bootstrap CSS -->
    <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.5.2/css/bootstrap.min.css"
        integrity="sha384-JcKb8q3iqJ61gNV9KGb8thSsNjpSL0n8PARn9HuZOnIxN0hoP+VmmDGMN5t9UJ0Z" crossorigin="anonymous">


    {% if title %}
    <title>Django Blog {{ title }}</title>
    {% else %}
    <title>Django Blog {{ title }}</title>
    {% endif %}

</head>

<body>
    <div class="container">
        {% block content %}{% endblock %}
    </div>


    <!-- Optional JavaScript -->
    <!-- jQuery first, then Popper.js, then Bootstrap JS -->
    <script src="https://code.jquery.com/jquery-3.5.1.slim.min.js"
        integrity="sha384-DfXdz2htPH0lsSSs5nCTpuj/zy4C+OGpamoFVy38MVBnE+IbbVYUew+OrCXaRkfj"
        crossorigin="anonymous"></script>
    <script src="https://cdn.jsdelivr.net/npm/popper.js@1.16.1/dist/umd/popper.min.js"
        integrity="sha384-9/reFTGAW83EW2RDu2S0VKaIzap3H66lZH81PoYlFhbGU+6BZp6G7niu735Sk7lN"
        crossorigin="anonymous"></script>
    <script src="https://stackpath.bootstrapcdn.com/bootstrap/4.5.2/js/bootstrap.min.js"
        integrity="sha384-B4gt1jrGC7Jh4AgTPSdUtOBvfO8shuf57BaghqFfPlYxofvL8/KUEfYiJOMMV+rV"
        crossorigin="anonymous"></script>
</body>

</html>
```

### Navbar

1.Download the [snippet folder](https://github.com/CoreyMSchafer/code_snippets/blob/master/Django_Blog/snippets)

2.Copy the code from navigation.html and paste in base.html file at below the body tag.

3.Create a static folder in Blog Application.Since  we need to add css which is a part of actual project which need to put it somewhere.In Django static file like css and js nees to be location in a static directory within our App

4.Similar add main.html

5.Create a Blog folder inside static folder

6.Add main.css from snippet folder

7.To add main.css we need to first load static at top in base.html {% load static %}

8.Add css file `<link rel="stylesheet" type="text/css" href=" {% static 'Blog/main.css' %}">` so the static statment does here it generates an absolute URL of the static files and acceses that Blog/main.

9.Restart the server and go to the server

10.Add article.html from snippet folder into home.views

11.We have some link here in our navigation bar

```html
<a class="navbar-brand mr-4" href="/">Django Blog</a> This link href="/"  goes to homepage.
<a class="nav-item nav-link" href="/">Home</a>
<a class="nav-item nav-link" href="/about">About</a> This link href="/about"  goes to About page.
<a class="nav-item nav-link" href="#">Login</a> This link href="#" are dead link
<a class="nav-item nav-link" href="#">Register</a>  
```

This link are hard code to these location and we know that we can easy change route in urls.py of application.So we have change href in multiple location ,.So in this case we can use django url tag in order to get absloute url to a given url pattern example {% url 'blog-home' %} check urls.py

`<a class="navbar-brand mr-4" href="{% url 'blog-home' %}">Django Blog</a>`
`<a class="nav-item nav-link" href="{% url 'blog-about' %}">About</a>`

## Admin

[admin](https://localhost:8000/admin)

### Creating super user

`python manage.py createsuperuser`

A error has come no such table: auth_user
This error means that we have not created database

`python manage.py makemigrations`

Output :-No changes detected

If we have created our own database tables or models then we would have seen changes there.Makemigrations just detects the changes and prepares django to update thr database but it doesn't actually run those changes yet in order to apply the migrations we use

`python manage.py migrate`

It will create tables for auth_user,etc.

`python manage.py createsuperuser`

<!-- user:-virajsaiya
pwd:-123
user:- TestUser
pwd:- testing321 -->

## Django ORM

ORM:
It stands for basic relational mapper and It is allows us to access our database and easy to use object oriented way and the things thai I like about it most is that you can use different databases without changing your code

The django ORM is that we can represent our database structure as classes and you'll hear those classes be called models.

Open model.py in blog.py

### Post class or model

1.Create a class called Post which inherit from models.Model
    So each class is going to be its own table in database and each attribute will be different field in database

2.Creating attributes and adding field for the post

`models.DateTimeField(auto_now=True)`f #Update date posted to Current date time every time post was updated.It would be great for last modified field

or

`models.DateTimeField(auto_now_add=True)` #this would set the date posted to the current date time only when this object is created.But in this case you can't ever update the value of th date posted so it will have to keep the exact date time of when the post was created.

or if you want to change date time we will use an argument default

``` python
        from django.utils import timezone
        date_posted = models.DateTimeField(default=timezone.now)
```

We are importing a timezone and this will takes our timezone settings into consideration so now we assign our default function timezone.now note we are not calling funtion now that the reason we are not paraentheses after timezone.now (timezone.now()) Since we dont want to excute that function at that point.

Now we need author for each post and this will be the user who created the post
Now our user is a separate table so first we need to import the User model(the admin where we create the user and superuser) and django created  that in the location

`from django.contrib.auth.models import User`

So the post model and User model have are going to have relation since user are going to author post specifically this is going to called a one to many relation ship because one user can have multiple post and post can have one author and do this in django we use a foreign key

A ForeignKey is use to connect database with each other so the post table in which author field will be connected to auth table User field or author column is reference to User column  
`author = models.ForeignKey(User,on_delete=models.CASCADE)`

on_delete we need to tell django what we want to do i the user who created this post gets deleted so if a user created post and then the user was deleted then do we want to delete the post or do we want to set it none or anything else,
But in our case we will delete all the post of user if user is deleted. So we will set on_delete to CASCADE where CASCADE will delete entire column associated with user.But that only one way streets so if post is deleted user won't be delete if user was delete it would be bad design.

So now we have update what our django database hold so we need to **Update database**  through running migrations command

`python manage.py makemigrations`

```cmd
(django_env) F:\Python Code\Django codes\django_project>python manage.py makemigrations
Migrations for 'Blog':
  Blog\migrations\0001_initial.py
    - Create model Post
```

![alt Error Message](Documentation_Images\image_6.jpg)
So will create our Post model or table and if you want to see SQl command

`python manage.py sqlmigrate Blog 0001`

```cmd
(django_env) ..\Django codes\django_project> python manage.py sqlmigrate Blog 0001
BEGIN;
--
-- Create model Post
--
CREATE TABLE "Blog_post" ("id" integer NOT NULL PRIMARY KEY AUTOINCREMENT, "title" varchar(100) NOT NULL, "content" text NOT NULL, "date_posted" datetime NOT NULL, "author_id" integer NOT NULL REFERENCES "auth_user" ("id") DEFERRABLE INITIALLY DEFERRED);
CREATE INDEX "Blog_post_author_id_89a23590" ON "Blog_post" ("author_id");
COMMIT;
```

`python manage.py migrate`

```cmd
(django_env) ..Django codes\django_project>python manage.py migrate
Operations to perform:
  Apply all migrations: Blog, admin, auth, contenttypes, sessions
Running migrations:
  Applying Blog.0001_initial... OK
```

### Why migrations are useful

because it alllows us to make changes to our database even after it's created and has data in that database.

So if we didn't have a way to run migration then we would have to run some cpmplicated SQL code to update our database structure so it didn't mess with the current data.

So now the changes have been add to the database

## How to query the database using these models

Now Django ORM let us do this through the classes as well
so to illustrate this we run django python shell which will allows us to work with these models interactively line by line so we can run the shell by using command

`python manage.py shell`

It look to be a python prompt and that exactly what it is we can run Python code in here but we can also work with django object.

For example let import both our post model and user model

We have created two user in admin Page we know that we already have two users so let query our user table.

To get all of the user we can simply say

`User.objects.all()`

Get first User

`User.objects.first()`

Get last User

`User.objects.last()`

filter based on any field

`User.objects.filter(username='testUser')` In this case we will recieve an empty set

`User.objects.filter(username='TestUser')`
This give s us a query set as a resultbut query set has only one user since the user name are unique now if that field wasn't unique then it possible filter could return multiple results.

So we could use the first method here as well just so we can grab first result of that filter

`User.objects.filter(username='TestUser').first()`

now we can see that we just get that user instead of the query set with the one user in query set

so now lets's actually take a look at this user that is getting returned so now we will store in a variable

`user= User.objects.filter(username='TestUser').first()`

So now we look attributes of this user

`user.id`

We can also use primary key to get the id

`user.pk`

GET
`user=User.objects.get(id=1)`
`user=User.objects.get(id=2)`

``` cmd
Python 3.8.2 (tags/v3.8.2:7b3ab59, Feb 25 2020, 23:03:10) [MSC v.1916 64 bit (AMD64)] on win32
Type "help", "copyright", "credits" or "license" for more information.
(InteractiveConsole)

>>>from Blog.models import Post
>>>from django.contrib.auth.models import User

>>>User.objects.all()
<QuerySet [<User: virajsaiya>, <User: TestUser>]>

>>>User.objects.first()
<User: virajsaiya>

>>>User.objects.last()
<User: TestUser>

>>>User.objects.filter(username='VirajSaiya')
<QuerySet []>

>>>User.objects.filter(username='TestUser')
<QuerySet [<User: TestUser>]>

>>> User.objects.filter(username='TestUser').first()
<User: TestUser>

>>> user= User.objects.filter(username='TestUser').first()
>>> user
<User: TestUser>
>>> user.id
2
>>> user.pk  
2
>>> user1= User.objects.filter(username='virajsaiya').first()
>>> user1
<User: virajsaiya>
>>> user1.id
1
>>> user1.pk
1

>>> user=User.objects.get(id=1)
>>> user
<User: virajsaiya>
>>> user=User.objects.get(id=2)
>>> user
<User: TestUser>
>>> user=User.objects.get(id=3)
Traceback (most recent call last):
  File "<console>", line 1, in <module>
  File "F:\Python Code\Django codes\django_env\lib\site-packages\django\db\models\manager.py", line 85, in manager_method
    return getattr(self.get_queryset(), name)(*args, **kwargs)
  File "F:\Python Code\Django codes\django_env\lib\site-packages\django\db\models\query.py", line 429, in get
    raise self.model.DoesNotExist(
django.contrib.auth.models.User.DoesNotExist: User matching query does not exist.

>>> user=User.objects.get(id=2)
>>> user.pk
2
>>> user=User.objects.get(id=1)
>>> user.pk
1
>>>
```

So now we have this user variable equal to this <User: virajsaiya>

So now let create a new post and make the user(virajsaiya) the author of this new post

First of all we shouldn't have any post right now so if I run a query then we get an empty query set

Let create some post written by this user

`post1 = Post(title='Blog 1',content='First Post Content! ',author=user)`
if you see that we haven't pass date for this post  now if you remember from our model we  have the default date of the current date time so it should populate with current date time.if we don't provide it anything

So now let query our post table again by re rum the `Post.objects.all()` we will get empty query and there is still no post that because we craeted the post object as this post_1 variable but we didn't actually save it to our database so to do that we can say `post1.save`.Now we will query our post object and table we get query set with one post object

```cmd
>>> user=User.objects.get(id=1)
>>> user
<User: virajsaiya>
>>> Post.objects.all()
<QuerySet []>
>>> post1 = Post(title='Blog 1',content='First Post Content! ',author=user)
>>> Post.objects.all()
<QuerySet []>
>>> post1.save()
>>> Post.objects.all()
<QuerySet [<Post: Post object (1)>]>
```

This Post object is not very descriptive so if you remember when we printed out our users the usernames of user where nice and descriptive

```cmd
>>> Post.objects.all()
<QuerySet [<Post: Post object (1)>]>

>>> User.objects.all()  
<QuerySet [<User: virajsaiya>, <User: TestUser>]>
```

To get post object to be moe discriptive we have to tell what we want to see what we printed out.
We can do that by dunder STR method (__str__)

Exit shell first
Open models.py in applications and Add this in class Post

```python
def __str__(self):
    return self.title
```

If we query our post Now you can see that we get a query set and the object says post of Blog 1 so now it is using that blog title in order to print that out.

assining user= User.objects.filter(username='virajsaiya').first() since we lost our variable due to closing shell.

Creating  a Second Post post_2

Now you can see that we have two Blog post

Now we will assign first postto a variable post

date_posted is automatically add  when ever create the post as we had assign default in models.py

If you see that when we called post.author it returns that user object and that actually entire user object so you could even access that user objects attributes so for example if i wanted this <User: virajsaiya> user email address  instead of rerunning another query or anything like that for example

```cmd
>>> AA=User.objects.first()
>>> AA.email
'saiyaviraj.09@gmail.com'
```

I could simply say post.author.email and now we can see the author email address and that really nice feature that we're able to get extra information from that foreign key like that.

Get post by specific user so we want all the post by virajsaiya now you might think that we need to fetch the user and then do a quey on the post model filtering by the post with that user as the author that definately one way to do it.But  Django adds special query set to the user model that allows us to do this a lot more easily and the naming convention for this query set is the name of the related model then underscore set so it would reference like this    user.modelname_set

So when it come to the our user the related model is named post so we can access the post of the user variable simply by saying
user here we get username User: virajsaiya with this username now if we want to get all of the posts that have written then this is `user.post_set` and we run that we see that it return something that isn't too readable but we can actually use that post set to run queries on the post that this user has created so if we wanted to get all of the posts then we could simply say    `user.post_set.all()`  and if we run that we can see that now we have a query set of the two post that we've created because this user is the author of both of those posts.

Now we can actually create a post directly using that post set so if I wanted to create a third  post directky using this user then I could simply say `user.post_set.create(title='Blog 3,content='Third Post Content!')` notice that we didn;t specify an author for that post because Django knows that we wanted to create that post for that users post set which means that it automatically make them author an we don't need to run .save or anything.It automatically saved that to database.

Now if we query `Post.objects.all()` we will get three post.

```cmd
from Blog.models import Post
from django.contrib.auth.models import user

>>> Post.objects.all()
<QuerySet [<Post: Blog 1>]

>>> user=User.objects.filter(username='virajsaiya').first()
>>> user
<User: virajsaiya>

>>> post_2 =Post(title='Blog 2',content='Second PostContent!',author_id = user.id)
>>> post_2.save()
>>> Post.objects.all()
<QuerySet [<Post: Blog 1>, <Post: Blog 2>]>

>>> post=Post.objects.first()

>>> post.content
'First Post Content! '

>>> post.date_posted
datetime.datetime(2020, 9, 16, 9, 45, 55, 386563, tzinfo=<UTC>)
>>> post.title
'Blog 1'

>>> post.author
<User: virajsaiya>

>>> post_2.author.email
'saiyaviraj.09@gmail.com

>>> user.post_set
<django.db.models.fields.related_descriptors.create_reverse_many_to_one_manager.<locals>.RelatedManager object at 0x000001B1A4315910>

>>> user.post_set.all()
<QuerySet [<Post: Blog 1>, <Post: Blog 2>]>

>>> user.post_set.create(title='Blog 3',content='Third Post Content!')
<Post: Blog 3>

>>> Post.objects.all()
<QuerySet [<Post: Blog 1>, <Post: Blog 2>, <Post: Blog 3>]>
```

Now we use the database query we just learned in order to use this real data that we've have added to our database instead of the dummy data that we have right now

Open view.py in Blog App

1.Import our post model
    (.models) dot in front of models layer just means from the models file in current package import our post class

2.Now in home function context instead of passing dummy data lets instead query all of our posts from database like we saw in the command line

```python
from .models import Post

def home(request):
    context = {'posts': Post.objects.all()}

    return render(request, 'Blog/home.html', context)


```

Now if your database field are different than the keys that you put here in your dummy data dictionary then you have to go to your templates and changes those accordingly.

How to change formatting of date

1.Open templates then home.html

`<small class="text-muted">{{ post.date_posted }}</small>`

Within our template tags there different filter that we can use to change our data around little bit so for date there is a date filter so to use this we can use the vertical bar '|' and then the filter

Se the document page 1372 in django for date filter or format
F:-Month, textual, long
d:-Day of the month, 2 digits with leading zeros
Y:-Year, 4 digits.
`<small class="text-muted">{{ post.date_posted|date:"F d Y" }}</small>`s

If you open [Admin](http://localhost:8000/admin)
You will see Groups and User but not Post so to see Post we first need to register our model with the admin page

Open admin.py in Blog App

```python
from django.contrib import admin
from .models import Post

admin.site.register(Post)
```

Now we can see the post and we can update our blog through it

## User Registration

We learn how to use forums and alos how to validate user input to create a user registration page so the application that we're creating is going to have the ability for users to create an account login make posts and things like that and the first part of that process is to create the registration page where a user can create an account on the website and then be able to log in and log out.  

The admin page we've seen might be good enough if you're the only person using your site but if you're making an application that anyone else can sign.Yoy need tocreate forms.

Think about what how user logic will relate to our project as a whole so the user account portion of our project is going to have its own forms and templates
and routes and things like that which is logically going to be sperate from blog itself so the best thing is to create a new app inide our project where all of that is going to be contained it owns section yhat way we know if we want to update our user accounts then well know exactly where to look we can just look in a users and a user app.

1.Create a new project users
    `python manage.py startapp users`

2.Create a user registration page where users can sign up create accounts from fontend of our website **Do not forget add our App inside project settings.py**

3.Open views.py file of users app
  Create a form that going to be passed to the template that we will create for this view.now if you were to create forms from scratch thenit can get pretty complicated pretty fasr so you'd have to put in different kinds of validation checks to make sure that the user was inserting information correctly make sure that they're hashed password matched you might have to write some regular expressions to make sure that there  that they enter a valid email and more complex stuff

  But using Django take care all of a lot of this stuff for us on the backend so this is kind of similar to the database models in the sense that we can  create Python classes and these classes generate html forms for us some classes already exist so for example we want a registration form so users can sign up for our site now in this case we can simply use the user creation form that already exists in Django so like I said these forms will be classes that will get converted into Html so  we can create our own form.

4.Creating a form for new user

1. This kind of form already exist in Django.We just need to import `from django.contrib.auth.forms import UserCreationForm`

2. Now we need to use this form in our register view. We just need to create instance of the form i.e `form=UserCreationForm()`

3. Render template to use this form

    ```python
    from django.shortcuts import render
    from django.contrib.auth.forms import UserCreationForm


    def register(request):
        form = UserCreationForm()
        return render(request, 'users/register.html', {'form': form})

    ```

4.Creating a template register.html (folder:users/templates/user/register.html)

1. You can see here that wer'e extending blog/base.html template so even within our users app we can reference templates form our blog app which is really nice

2. Add a div with a class called content-section which a css and form with method Post

3. Add CSRF(cross site request  forgery) token and it a hidden tag which will protect our form from certain attacks so just add security it complusary in form otherwise it won't work

4. Adding fieldset tag which group related element in a form and also legend for our forms.

5. Inside fieldset we will assign a bootstrap class form-group and also we will assign a bootstrap class called boder-botttom md-4 (md=margin bottom)

6. Now we need to print out our form so we can simply acces that form variable(In view.py) passed in to context and thaat should print out entire form.So just specifying that form variable will display all of the form label and field that we need.

7. Adding Sign up button with some bootstrap btn btn-outline-info and also what  type of button should submit after fieldset.

8. Add div for Sign In user outide the form with bootstrap class and small(small text for eg copyright) tag [text-muted a class of dull text]

    ```html
    {% extends "Blog/base.html"%}
    {% block content %}
    <div class="content-section">
        <form method="POST">
            {% csrf_token %}
            <fieldset class="form-group">
                <legend class="boder-bottom md-4">Join Today</legend>
                {{ form }}
            </fieldset>
            <div class="from-group">
                <button class="btn btn-outline-info type=" submit">Sign Up</button>
            </div>
        </form>
        <div class="boder-top pt-3">
            <small class="text muted">Already Have an Account? <a href="#">Sign In</a>
            </small>
        </div>
    </div>
    {% endblock content %}
    ```

9. Add url inside our project url.py directly nOTE editor will show you the error since python  

    ```python
    from django.contrib import admin
    from django.urls import path, include
    from users import views as user_views
    urlpatterns = [
        path('admin/', admin.site.urls),
        path('register/', user_views.register, name='register'),
        path('', include('Blog.urls')),
    ]

    ```

10. Rendering templates in forms

    When we render our form template the password is not in single that because it is render by default as table

    ![alt Render form as table](Documentation_Images\image_7.jpg)

    So we need to render our form as paragraph tags which will show password field on next line
    So we need to pass our input `{{ form }}` as {{ form.as_p }}\

    ```html
    {% extends "Blog/base.html"%}
    {% block content %}
    <div class="content-section">
        <form method="POST">
            {% csrf_token %}
            <fieldset class="form-group">
                <legend class="boder-bottom md-4">Join Today</legend>
                {{ form.as_p }}
            </fieldset>
            <div class="from-group">
                <button class="btn btn-outline-info type=" submit">Sign Up</button>
            </div>
        </form>
        <div class="boder-top pt-3">
            <small class="text muted">Already Have an Account? <a href="#">Sign In</a>
            </small>
        </div>
    </div>
    {% endblock content %}
    ```

    ![alt Render form as paragraph](Documentation_Images\image_8.jpg)

    Reference

    ![alt Reference](Documentation_Images\image_9.jpg)

Currently went we hit sign up button it wipe out information in field of pswd and username and redirect to same page.If we see our admin page no new user were created.The reason is because this is  performing a Post request on our register view(view.py) route with form information submitted but we are not doing anything with the information.If we see our register view(users app) that any request that comes to this route were are simply creating a blank form and rendering it out to the template.
