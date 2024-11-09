<<<<<<< HEAD
# blog_app

A new Flutter project.

## Getting Started

This project is a starting point for a Flutter application.

A few resources to get you started if this is your first Flutter project:

- [Lab: Write your first Flutter app](https://docs.flutter.dev/get-started/codelab)
- [Cookbook: Useful Flutter samples](https://docs.flutter.dev/cookbook)

For help getting started with Flutter development, view the
[online documentation](https://docs.flutter.dev/), which offers tutorials,
samples, guidance on mobile development, and a full API reference.
=======
# Blog App

A mobile-friendly blog app built with a Django REST API backend and a Flutter frontend. This app displays a list of blog posts, allows users to view detailed content for each post, and is designed with an elegant black-and-wheat theme.

## Table of Contents

1. [Project Overview](#project-overview)
2. [Features](#features)
3. [Backend (Django) Setup](#backend-django-setup)
   - [Requirements](#requirements)
   - [Setting up the Django API](#setting-up-the-django-api)
   - [API Endpoints](#api-endpoints)
4. [Frontend (Flutter) Setup](#frontend-flutter-setup)
   - [Requirements](#requirements-1)
   - [Project Structure](#project-structure)
   - [Theming and Styling](#theming-and-styling)
5. [Running the Application](#running-the-application)
6. [Screenshots](#screenshots)

---

## Project Overview

The Blog App is designed to display a list of blog posts with smooth navigation between pages using Flutter’s navigation features. The Django backend provides a REST API for fetching blog data.

## Features

- **Blog List Page**: Displays blog titles and descriptions in a scrollable list.
- **Blog Detail Page**: Displays the full content of a blog post.
- **Smooth Navigation**: Allows easy transitions between list and detail views.
- **Dark Theme**: Black-and-wheat color scheme for a sleek and modern look.
- **Responsive UI**: Designed to work well across devices.

## Backend (Django) Setup

### Requirements

- Python 3.x
- Django 3.x or later
- Django REST framework
- django-cors-headers (for handling CORS in development)

### Setting up the Django API

1. **Install Django and Django REST framework**:
   ```bash
   pip install django djangorestframework django-cors-headers
   ```

2. **Create a Django project and app**:
   ```bash
   django-admin startproject blog_backend
   cd blog_backend
   python manage.py startapp blog
   ```

3. **Configure Django Settings**:

   In `settings.py`, add `rest_framework` and `corsheaders` to `INSTALLED_APPS` and configure `CORS_ALLOW_ALL_ORIGINS` for development:

   ```python
   INSTALLED_APPS = [
       'rest_framework',
       'corsheaders',
       'blog',
       ...
   ]

   MIDDLEWARE = [
       'corsheaders.middleware.CorsMiddleware',
       ...
   ]

   CORS_ALLOW_ALL_ORIGINS = True  # Allow all origins for development
   ```

4. **Define the Blog Model** in `blog/models.py`:

   ```python
   from django.db import models

   class Blog(models.Model):
       title = models.CharField(max_length=200)
       description = models.TextField()
       content = models.TextField()
       created_at = models.DateTimeField(auto_now_add=True)

       def __str__(self):
           return self.title
   ```

5. **Create API Serializers** in `blog/serializers.py`:

   ```python
   from rest_framework import serializers
   from .models import Blog

   class BlogSerializer(serializers.ModelSerializer):
       class Meta:
           model = Blog
           fields = ['id', 'title', 'description', 'content', 'created_at']
   ```

6. **Create API Views** in `blog/views.py`:

   ```python
   from rest_framework import generics
   from .models import Blog
   from .serializers import BlogSerializer

   class BlogList(generics.ListAPIView):
       queryset = Blog.objects.all()
       serializer_class = BlogSerializer

   class BlogDetail(generics.RetrieveAPIView):
       queryset = Blog.objects.all()
       serializer_class = BlogSerializer
   ```

7. **Define URL Patterns** in `blog/urls.py`:

   ```python
   from django.urls import path
   from . import views

   urlpatterns = [
       path('blogs/', views.BlogList.as_view(), name='blog-list'),
       path('blogs/<int:pk>/', views.BlogDetail.as_view(), name='blog-detail'),
   ]
   ```

8. **Include Blog URLs in Project’s `urls.py`** in `blog_backend/urls.py`:

   ```python
   from django.contrib import admin
   from django.urls import path, include

   urlpatterns = [
       path('admin/', admin.site.urls),
       path('api/', include('blog.urls')),
   ]
   ```

9. **Run Migrations and Start the Server**:
   ```bash
   python manage.py makemigrations
   python manage.py migrate
   python manage.py runserver
   ```

### API Endpoints

- **List of Blogs**: `GET /api/blogs/`
- **Blog Detail**: `GET /api/blogs/<id>/`

## Frontend (Flutter) Setup

### Requirements

- Flutter SDK 2.x or later
- `http` package for API calls
- `animations` package for transition effects

### Project Structure

Organize the Flutter project as follows:

```
lib/
├── main.dart
├── models/
│   └── blog.dart
├── services/
│   └── api_service.dart
├── pages/
│   ├── blog_list_page.dart
│   └── blog_detail_page.dart
└── widgets/
    └── blog_card.dart
```

### Theming and Styling

- **Primary Colors**: Black background with wheat-colored accents.
- **Text Style**: Large and medium-sized text for readability.
- **Animations**: Smooth transitions using `PageTransitionSwitcher` and `FadeThroughTransition`.

**main.dart** - Sets the theme and initializes the app.

```dart
void main() {
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Blog App',
      theme: ThemeData(
        primaryColor: Colors.black,
        colorScheme: ColorScheme.fromSwatch().copyWith(
          secondary: Colors.amber[700],
          primary: Colors.black,
        ),
        scaffoldBackgroundColor: Colors.black,
        appBarTheme: AppBarTheme(
          backgroundColor: Colors.black,
          titleTextStyle: TextStyle(
            color: Colors.amber[700],
            fontSize: 20,
            fontWeight: FontWeight.bold,
          ),
        ),
        textTheme: TextTheme(
          bodyLarge: TextStyle(color: Colors.white70),
          bodyMedium: TextStyle(color: Colors.white70),
          titleLarge: TextStyle(
            color: Colors.amber[700],
            fontSize: 24,
            fontWeight: FontWeight.bold,
          ),
        ),
        cardColor: Colors.grey[900],
      ),
      home: BlogListPage(),
    );
  }
}
```

## Running the Application

1. **Run the Django Backend**:
   ```bash
   python manage.py runserver
   ```

2. **Run the Flutter App**:
   ```bash
   flutter run -d chrome
   ```

### CORS Configuration

To allow the Flutter web app to access the Django backend, we used the `django-cors-headers` package to enable CORS headers.

## Screenshots

Include screenshots for:

- **Blog List Page**: Showing the list of blog posts.
- ![image](https://github.com/user-attachments/assets/6168d62f-2756-4b86-a87f-49c61f6a6f35)

- **Blog Detail Page**: Displaying the full content of a selected blog post.

---![blog pic](https://github.com/user-attachments/assets/5e097ff0-d1a2-4f07-96ae-a21f65906e1b)
This is because no blogs have been uploaded yet


>>>>>>> 632aba7925323dae4f47719bc8877569b0c0e2fd
