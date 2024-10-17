# Challenge: Task Management System

## Description

Create a simple task management system where users can create, list, and delete tasks. The focus is on basic CRUD functionality, allowing users to manage their tasks effectively.

This challenge will test your understanding of user management and authentication in Django.

## Challenge Objective

- Create a `Task` model with fields such as `title`, `description`, `completed`, and `created_at`.
- Implement views (or API endpoints) for:
  - Creating tasks
  - Listing all tasks
  - Deleting tasks
- Handle simple validation for task creation.

## Project Structure Example

Here’s an example structure of your project:

```
    task_manager/
    ├── tasks/
    │   ├── models.py
    │   ├── serializers.py
    │   ├── views.py
    │   └── urls.py
    ├── task_manager/
    │   └── settings.py
    └── manage.py
```

## Step-by-Step Guide

1. **Create the `Task` Model:**:

   Define the `Task` model in `models.py` to represent the task entity.

```python
   from django.db import models

   class Task(models.Model):
       title = models.CharField(max_length=200)
       description = models.TextField(blank=True, null=True)
       completed = models.BooleanField(default=False)
       created_at = models.DateTimeField(auto_now_add=True)

       def __str__(self):
           return self.title
```

2. Create the Serializer:
   Create a `TaskSerializer` in `serializers.py` to handle task data serialization.

```python
    from rest_framework import serializers
    from .models import Task

    class TaskSerializer(serializers.ModelSerializer):
        class Meta:
            model = Task
            fields = ['id', 'title', 'description', 'completed', 'created_at']
```

3. Create the Views:
   In `views.py`, define the views for creating, listing, and deleting tasks.

```python
    from rest_framework.decorators import api_view
    from rest_framework.response import Response
    from rest_framework import status
    from .models import Task
    from .serializers import TaskSerializer

    @api_view(['GET', 'POST'])
    def task_list_create(request):
        if request.method == 'GET':
            tasks = Task.objects.all()
            serializer = TaskSerializer(tasks, many=True)
            return Response(serializer.data)

        elif request.method == 'POST':
            serializer = TaskSerializer(data=request.data)
            if serializer.is_valid():
                serializer.save()
                return Response(serializer.data, status=status.HTTP_201_CREATED)
            return Response(serializer.errors, status=status.HTTP_400_BAD_REQUEST)

    @api_view(['DELETE'])
    def task_delete(request, pk):
        try:
            task = Task.objects.get(pk=pk)
            task.delete()
            return Response(status=status.HTTP_204_NO_CONTENT)
        except Task.DoesNotExist:
            return Response(status=status.HTTP_404_NOT_FOUND)
```

4. Create the URL Routes:
   Define the URL routes for listing, creating, and deleting tasks in `urls.py`.

```python
    from django.urls import path
    from . import views

    urlpatterns = [
        path('tasks/', views.task_list_create, name='task-list-create'),
        path('tasks/<int:pk>/delete/', views.task_delete, name='task-delete'),
    ]
```

## Validation

- Ensure that the `title` field is filled during task creation.
- Validate that the `title` is unique and not empty.
- Provide meaningful error messages if the required fields are missing.

## User Feedback

- Provide clear messages when a task is created or deleted successfully.
- Return detailed error messages when validation fails. For example:
  - "Title field is required."
  - "Task not found." for delete requests.

## Tips

- Use tools like Postman or Insomnia to test the API endpoints.
- Ensure that your responses are consistent in structure, returning error messages in a clear format.
- Focus on completing the CRUD functionality in a simple and efficient manner.

## Final Considerations

These challenges are designed to test your understanding of Django models, views, and serializers. Keep the implementation simple and ensure the basic CRUD operations work as expected. Aim for clean and maintainable code.
