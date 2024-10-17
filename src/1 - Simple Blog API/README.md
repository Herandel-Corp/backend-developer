# Challenge: Simple Blog API

## Description

Create a basic REST API for a blog using **Django** and **Django REST Framework**. You will need to set up two endpoints:

- One to create a blog post.
- Another to list all blog posts.

This challenge tests your ability to create models and API endpoints using Django. You will need to handle basic validation and serialization.

## Challenge Objective

- Create a `Post` model with fields such as `title`, `content`, and `created_at`.
- Set up the required API endpoints using **Django REST Framework**.
- Handle basic data validation, such as ensuring that required fields are not empty.

## Project Structure Example

Here’s an example structure of your project:

```
blog_api/
├── blog/
│   ├── models.py
│   ├── serializers.py
│   ├── views.py
│   └── urls.py
├── blog_api/
│   └── settings.py
└── manage.py
```

## Step-by-Step Guide

1. **Create the `Post` Model**:

   In `models.py`, define the `Post` model with basic fields.

```python
   from django.db import models

   class Post(models.Model):
       title = models.CharField(max_length=200)
       content = models.TextField()
       created_at = models.DateTimeField(auto_now_add=True)

       def __str__(self):
           return self.title
```

2. Create the Serializer:

   In `serializers.py`, create a serializer to convert the model into JSON format.

```python
   from rest_framework import serializers
   from .models import Post

   class PostSerializer(serializers.ModelSerializer):
   class Meta:
   model = Post
   fields = ['id', 'title', 'content', 'created_at']
```

3. Create the Views:

   In `views.py`, define the views for creating and listing posts using **Django REST Framework**.

```python

   from rest_framework.response import Response
   from rest_framework.decorators import api_view
   from .models import Post
   from .serializers import PostSerializer

   @api_view(['GET', 'POST'])
   def post_list_create(request):
   if request.method == 'GET':
   posts = Post.objects.all()
   serializer = PostSerializer(posts, many=True)
   return Response(serializer.data)

       elif request.method == 'POST':
           serializer = PostSerializer(data=request.data)
           if serializer.is_valid():
               serializer.save()
               return Response(serializer.data, status=201)
           return Response(serializer.errors, status=400)
```

4. Create the URL Routes:

   In `urls.py`, set up the routes for the API.

```python
    from django.urls import path
    from . import views

    urlpatterns = [
    path('posts/', views.post_list_create, name='post-list-create'),
    ]
```

## Validation

- Ensure that the title and content fields are not empty.
- Provide meaningful error messages if validation fails.

## User Feedback

- Upon successful creation, return the newly created post data and a success message.
- If there are validation errors, return a 400 status code with detailed error messages.

## Tips

- Use tools like Postman or Insomnia to test your API endpoints.
- Focus on completing the task within the allocated time and ensure that the core functionality works properly.

## Final Considerations

This challenge is designed to test your understanding of Django models, views, and serializers. Keep the implementation simple and ensure the basic functionality works as expected.
