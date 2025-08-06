
# Electrical Machines Q&A Platform

A comprehensive web application built with Django and MySQL for electrical engineering students and professionals to ask questions about electrical machines and receive AI-powered answers.

## Features

- **User Authentication**: Complete registration and login system
- **Question Management**: Ask, view, and search electrical machines questions
- **AI-Powered Answers**: Integration with ChatGPT for expert responses
- **Responsive Design**: Mobile-friendly interface without Bootstrap
- **Category System**: Organized by DC Machines, AC Machines, Transformers, etc.
- **User Profiles**: Personal dashboards with question history
- **Search Functionality**: Find questions by keywords and categories

## Technology Stack

- **Backend**: Django 4.2.7
- **Database**: MySQL
- **AI Integration**: OpenAI ChatGPT API
- **Frontend**: HTML5, CSS3, JavaScript (Vanilla)
- **Deployment**: Ubuntu Server with Gunicorn

## Project Structure

```
electrical_machines_qa/
├── electrical_machines_qa/    
│   ├── settings.py           
│   ├── urls.py             
│   ├── wsgi.py              
│   └── asgi.py              
├── qa_app/                   
│   ├── models.py           
│   ├── views.py             
│   ├── forms.py             
│   ├── urls.py              
│   ├── admin.py            
│   ├── chatgpt_service.py   
│   └── migrations/          
├── templates/                
│   ├── base.html            
│   ├── home.html            
│   ├── login.html           
│   ├── register.html        
│   ├── ask_question.html    
│   ├── question_detail.html 
│   └── profile.html         
├── static/                   
│   ├── css/style.css        
│   └── js/main.js           
├── requirements.txt          
├── manage.py                
└── deployment_guide.md      
```

## Installation and Setup

### Prerequisites

- Python 3.8+
- MySQL 8.0+
- Ubuntu 20.04+ (for deployment)

### Local Development Setup

1. **Clone the repository**
   ```bash
   git clone <repository-url>
   cd electrical_machines_qa
   ```

2. **Create virtual environment**
   ```bash
   python3 -m venv venv
   source venv/bin/activate  # On Windows: venv\Scripts\activate
   ```

3. **Install dependencies**
   ```bash
   pip install -r requirements.txt
   ```

4. **Setup MySQL database**
   ```sql
   CREATE DATABASE electrical_machines_qa;
   CREATE USER 'qa_user'@'localhost' IDENTIFIED BY 'secure_password';
   GRANT ALL PRIVILEGES ON electrical_machines_qa.* TO 'qa_user'@'localhost';
   FLUSH PRIVILEGES;
   ```
   
# electrical_machines_qa/settings.py

import os
from pathlib import Path
from dotenv import load_dotenv

load_dotenv()

BASE_DIR = Path(__file__).resolve().parent.parent

SECRET_KEY = os.getenv("SECRET_KEY", "dummy-secret-key")
DEBUG = os.getenv("DEBUG", "True") == "True"
ALLOWED_HOSTS = ["*"]  # Change this in production
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'qa_app',  # Main app
]

MIDDLEWARE = [
    'django.middleware.security.SecurityMiddleware',
    'django.contrib.sessions.middleware.SessionMiddleware',
    'django.middleware.common.CommonMiddleware',
    'django.middleware.csrf.CsrfViewMiddleware',
    'django.contrib.auth.middleware.AuthenticationMiddleware',
    'django.contrib.messages.middleware.MessageMiddleware',
    'django.middleware.clickjacking.XFrameOptionsMiddleware',
]

ROOT_URLCONF = 'electrical_machines_qa.urls'

TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [BASE_DIR / "templates"],
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

WSGI_APPLICATION = 'electrical_machines_qa.wsgi.application'

DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': os.getenv("DB_NAME"),
        'USER': os.getenv("DB_USER"),
        'PASSWORD': os.getenv("DB_PASSWORD"),
        'HOST': os.getenv("DB_HOST", "localhost"),
        'PORT': os.getenv("DB_PORT", "3306"),
        'OPTIONS': {
            'init_command': "SET sql_mode='STRICT_TRANS_TABLES'",
        },
    }
}

AUTH_PASSWORD_VALIDATORS = [
    {'NAME': 'django.contrib.auth.password_validation.UserAttributeSimilarityValidator'},
    {'NAME': 'django.contrib.auth.password_validation.MinimumLengthValidator'},
    {'NAME': 'django.contrib.auth.password_validation.CommonPasswordValidator'},
    {'NAME': 'django.contrib.auth.password_validation.NumericPasswordValidator'},
]

LANGUAGE_CODE = 'en-us'
TIME_ZONE = 'Asia/Kolkata'
USE_I18N = True
USE_L10N = True
USE_TZ = True

STATIC_URL = '/static/'
STATICFILES_DIRS = [BASE_DIR / "static"]
STATIC_ROOT = BASE_DIR / "staticfiles"

MEDIA_URL = '/media/'
MEDIA_ROOT = BASE_DIR / 'media'

DEFAULT_AUTO_FIELD = 'django.db.models.BigAutoField'

# OpenAI API
OPENAI_API_KEY = os.getenv("OPENAI_API_KEY")

6. **Environment variables**
   Create a `.env` file in the project root:
   ```env
   SECRET_KEY=your-django-secret-key-here
   DEBUG=True
   DB_NAME=electrical_machines_qa
   DB_USER=qa_user
   DB_PASSWORD=secure_password
   DB_HOST=localhost
   DB_PORT=3306
   OPENAI_API_KEY=your-openai-api-key-here
   ```

7. **Run migrations**
   ```bash
   python manage.py makemigrations
   python manage.py migrate
   ```

8. **Create superuser**
   ```bash
   python manage.py createsuperuser
   ```

9. **Run development server**
   ```bash
   python manage.py runserver
   ```
