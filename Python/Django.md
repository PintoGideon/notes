Django was invented to meet fast moving newsroom deadlines while satisfying the tough requirements of experienced Web Developers

```python
python3 -m venv name_of_local_environment
django-admin startproject mysite .
ls
local_env manage.py mysite quotes
python3 manage.py migrate
python3 manage.py runserver


```

### Django

Mapping url patterns to views

```python
# In views.py

from django.http import HttpResponse

def home(request):
    return HttpResponse("<h1>Hey there</h1>")
```

```python
# In urls.py

from django.conf.urls import url
from django.contrib import admin

from .views import home

urlpatterns=[
    url(r'^$',home,name='Home')
]
```

Telling the browser to treat the response as a file attachment.
Use the content_type argument and set the Content-Disposition
header. For example, this how you return a MS excel spreadsheet.

```python
response=HttpResponse(my_data, content_type='application/vnd.ms-secel)
response['Content-Disposition']='attachment; filename="foo.xls"'
```

Most of the times, we are going to have Create, Retrieve, Update, Delete and List as views for our apps.

### Creating a List view

Rendering templates.

We can create a directory to store our templates.
template_path in the code below stores the path
to the list_view template.

```python
# In view.py

def post_model_list_view(request):
   qs= PostModel.objects.all()
   print(qs)
   template_path="list_view.html"
   context={}

   return render(request,template_path,context)


##3 In urls.py

from .views import post_model_list_view

urlpatterns=[
    url(r'blog/',include('blog.urls'))
]

```

### Understanding Context

```python

context={
    'object_list':qs
}

### In list_view.html

<h1>Displaying the context dictionary</h1>

{% for object in object_list %}
<li>
{{object.slug}}{object.id}{object.title}}
</li>
{% endfor %}

```

The login required decorator is used when
the user has to login to view.

```python
# in views.py

from django.contrib.auth.decorators import login_required


@login_required(login_url='/login')
def post_model_list_view(request):
 # print(request.user)
qs= PostModel.objects.all()
context={
    'object_list':qs
}

if request.user.isAuthenticated():
  template='list-view.html'
else:
    template="list-view-public.html
```
