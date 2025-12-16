# Django Essential Tools & Resources
A curated collection of high-value Django packages, UI frameworks, and utilities to streamline development, enhance admin interfaces, and enforce best practices in Django projects.

---

## Table of Contents
- [Admin Interface Enhancements](#admin-interface-enhancements)
- [Development Utilities](#development-utilities)
- [Environment & Code Quality](#environment--code-quality)
- [Project Maintenance & Security](#project-maintenance--security)
- [Database Management](#database-management)
- [Additional Resources](#additional-resources)

---

## Admin Interface Enhancements
### 1. Django Jazzmin
**Link**: [https://django-jazzmin.readthedocs.io/](https://django-jazzmin.readthedocs.io/)  
**Purpose**: Responsive, customizable admin interface with Bootstrap 5 support, dark mode, and brand styling.  

#### Full Installation & Setup
1. Install via pip:
   ```bash
   pip install django-jazzmin
   ```
2. Update `INSTALLED_APPS` in your Django `settings.py` (add **before** `django.contrib.admin`):
   ```python
   INSTALLED_APPS = [
       'jazzmin',  # Must come before django.contrib.admin
       'django.contrib.admin',
       'django.contrib.auth',
       'django.contrib.contenttypes',
       'django.contrib.sessions',
       'django.contrib.messages',
       'django.contrib.staticfiles',
       # Your other apps
   ]
   ```
3. (Optional) Add Jazzmin custom settings to `settings.py` (e.g., theme, icons):
   ```python
   JAZZMIN_SETTINGS = {
       "site_title": "My Django Admin",
       "site_header": "My Project",
       "site_brand": "My App",
       "dark_mode_theme": "darkly",
       "light_mode_theme": "flatly",
   }
   ```
4. Run migrations (if required):
   ```bash
   python manage.py migrate
   ```
5. Restart your development server:
   ```bash
   python manage.py runserver
   ```

### 2. Django Jet Reboot
**Link**: [https://pypi.org/project/django-jet-reboot/](https://pypi.org/project/django-jet-reboot/)  
**Purpose**: Modernized fork of the classic `django-jet` admin theme (maintained for Django 3.0+).  

#### Full Installation & Setup
1. Install via pip:
   ```bash
   pip install django-jet-reboot
   ```
2. Update `INSTALLED_APPS` in `settings.py` (before `django.contrib.admin`):
   ```python
   INSTALLED_APPS = [
       'jet',  # django-jet-reboot
       'django.contrib.admin',
       # Your other apps
   ]
   ```
3. Add Jet URL patterns to your project’s `urls.py`:
   ```python
   from django.urls import path, include
   from django.contrib import admin

   urlpatterns = [
       path('jet/', include('jet.urls', 'jet')),  # Jet URLS
       path('jet/dashboard/', include('jet.dashboard.urls', 'jet-dashboard')),  # Jet dashboard
       path('admin/', admin.site.urls),
       # Your other URLs
   ]
   ```
4. Collect static files (required for static assets like CSS/JS):
   ```bash
   python manage.py collectstatic
   ```
5. Run migrations:
   ```bash
   python manage.py migrate
   ```
6. Restart the dev server:
   ```bash
   python manage.py runserver
   ```

### 3. Django Unfold
**Link**: [https://pypi.org/project/django-unfold/](https://pypi.org/project/django-unfold/)  
**Purpose**: Clean, minimal admin theme with Tailwind CSS integration and developer-friendly customization.  

#### Full Installation & Setup
1. Install via pip:
   ```bash
   pip install django-unfold
   ```
2. Update `INSTALLED_APPS` in `settings.py` (before `django.contrib.admin`):
   ```python
   INSTALLED_APPS = [
       'unfold',  # Unfold theme
       'unfold.contrib.filters',  # Optional: Advanced filters
       'unfold.contrib.forms',    # Optional: Enhanced forms
       'django.contrib.admin',
       # Your other apps
   ]
   ```
3. (Optional) Add Unfold custom settings to `settings.py`:
   ```python
   UNFOLD = {
       "SITE_TITLE": "My Unfold Admin",
       "SITE_HEADER": "My Project",
       "SITE_LOGO": "/static/logo.png",  # Path to your logo
       "DARK_MODE": True,
   }
   ```
4. Collect static files:
   ```bash
   python manage.py collectstatic
   ```
5. Restart the dev server:
   ```bash
   python manage.py runserver
   ```

---

## Development Utilities
### 1. Django Extensions
**Link**: [https://pypi.org/project/django-extensions/](https://pypi.org/project/django-extensions/)  
**Purpose**: A suite of useful management commands and utilities for Django development.  

#### Full Installation & Setup
1. Install via pip:
   ```bash
   pip install django-extensions
   ```
2. Update `INSTALLED_APPS` in `settings.py`:
   ```python
   INSTALLED_APPS = [
       # Core Django apps
       'django.contrib.admin',
       'django.contrib.auth',
       # ...
       'django_extensions',  # Add this line
       # Your other apps
   ]
   ```
3. Verify installation (run a Django Extensions command):
   ```bash
   # Enhanced interactive shell (auto-imports models)
   python manage.py shell_plus

   # Generate ER diagram of your models (requires graphviz)
   python manage.py graph_models -a -o my_project_models.png
   ```
4. (Optional) Install optional dependencies (for extra features):
   ```bash
   # For graph_models (ER diagrams)
   pip install graphviz
   # For runserver_plus (enhanced dev server)
   pip install werkzeug
   ```

### 2. python-dotenv
**Link**: [https://pypi.org/project/python-dotenv/](https://pypi.org/project/python-dotenv/)  
**Purpose**: Load environment variables from a `.env` file (avoids hardcoding secrets in settings).  

#### Full Installation & Setup
1. Install via pip:
   ```bash
   pip install python-dotenv
   ```
2. Create a `.env` file in your project root (same directory as `manage.py`):
   ```env
   # .env file
   SECRET_KEY=your_secure_django_secret_key_here
   DEBUG=False
   DATABASE_URL=postgres://user:password@localhost:5432/my_db
   API_KEY=your_external_api_key_here
   ```
3. Update `settings.py` to load the `.env` file:
   ```python
   # At the top of settings.py
   import os
   from dotenv import load_dotenv

   # Load variables from .env file
   load_dotenv()  # Default: loads .env from project root

   # Replace hardcoded values with env vars
   SECRET_KEY = os.getenv('SECRET_KEY')
   DEBUG = os.getenv('DEBUG', 'False') == 'True'  # Default to False
   DATABASES = {
       'default': {
           'ENGINE': 'django.db.backends.postgresql',
           'NAME': os.getenv('DB_NAME'),
           'USER': os.getenv('DB_USER'),
           'PASSWORD': os.getenv('DB_PASSWORD'),
           'HOST': os.getenv('DB_HOST', 'localhost'),
           'PORT': os.getenv('DB_PORT', '5432'),
       }
   }
   ```
4. Add `.env` to `.gitignore` (**critical** to avoid committing secrets):
   ```gitignore
   # .gitignore
   .env
   __pycache__/
   *.pyc
   db.sqlite3
   media/
   static/
   ```

### 3. Black (Code Formatter)
**Link**: [https://pypi.org/project/black/](https://pypi.org/project/black/)  
**Purpose**: Opinionated code formatter for Python (enforces consistent style in Django projects).  

#### Full Installation & Setup
1. Install via pip:
   ```bash
   pip install black
   ```
2. Basic usage (format a single file/directory):
   ```bash
   # Format a single file
   black my_app/views.py

   # Format entire project
   black .
   ```
3. (Optional) Integrate with pre-commit hooks (auto-format before commits):
   - Install pre-commit:
     ```bash
     pip install pre-commit
     ```
   - Create a `.pre-commit-config.yaml` file in your project root:
     ```yaml
     repos:
       - repo: https://github.com/psf/black
         rev: 24.8.0
         hooks:
           - id: black
             language_version: python3.11  # Match your Python version
     ```
   - Install pre-commit hooks:
     ```bash
     pre-commit install
     ```
   - Now, Black will auto-format code every time you run `git commit`.

---

## Project Maintenance & Security
### 1. Django Hijack
**Link**: [https://django-hijack.readthedocs.io/en/stable/](https://django-hijack.readthedocs.io/en/stable/)  
**Purpose**: Safely impersonate other users in the admin panel (for debugging/support).  

#### Full Installation & Setup
1. Install via pip:
   ```bash
   pip install django-hijack
   ```
2. Update `INSTALLED_APPS` in `settings.py`:
   ```python
   INSTALLED_APPS = [
       # Core apps
       'django.contrib.admin',
       # ...
       'hijack',  # Add this line
       'hijack.contrib.admin',  # For admin integration
       # Your other apps
   ]
   ```
3. Add Hijack middleware to `settings.py` (after `AuthenticationMiddleware`):
   ```python
   MIDDLEWARE = [
       'django.middleware.security.SecurityMiddleware',
       'django.contrib.sessions.middleware.SessionMiddleware',
       'django.middleware.common.CommonMiddleware',
       'django.middleware.csrf.CsrfViewMiddleware',
       'django.contrib.auth.middleware.AuthenticationMiddleware',
       'hijack.middleware.HijackUserMiddleware',  # Add this line
       # ... other middleware
   ]
   ```
4. Add Hijack URL patterns to `urls.py`:
   ```python
   from django.urls import path, include
   from django.contrib import admin

   urlpatterns = [
       path('admin/', admin.site.urls),
       path('hijack/', include('hijack.urls')),  # Add this line
       # Your other URLs
   ]
   ```
5. (Optional) Configure Hijack permissions (restrict to superusers):
   ```python
   # settings.py
   HIJACK_AUTHORIZE_STAFF = False  # Only superusers can hijack
   HIJACK_LOGIN_REDIRECT_URL = '/admin/'  # Redirect after hijack
   HIJACK_LOGOUT_REDIRECT_URL = '/admin/auth/user/'  # Redirect after releasing hijack
   ```
6. Run migrations:
   ```bash
   python manage.py migrate
   ```
7. Verify in admin: Navigate to the User list → Click "Hijack" next to a user to impersonate them.

### 2. Django Cleanup
**Link**: [https://pypi.org/project/django-cleanup/](https://pypi.org/project/django-cleanup/)  
**Purpose**: Automatically delete orphaned media files (e.g., when a model instance with a `FileField`/`ImageField` is deleted).  

#### Full Installation & Setup
1. Install via pip:
   ```bash
   pip install django-cleanup
   ```
2. Update `INSTALLED_APPS` in `settings.py`:
   ```python
   INSTALLED_APPS = [
       # Core apps
       'django.contrib.admin',
       # ...
       'django_cleanup.apps.CleanupConfig',  # Add this line (exact path)
       # Your other apps
   ]
   ```
3. (Optional) Configure advanced settings (e.g., ignore certain models):
   ```python
   # settings.py
   DJANGO_CLEANUP_EXCLUDE = ('my_app.MyModel',)  # Skip cleanup for this model
   DJANGO_CLEANUP_DELETE_EXISTING = True  # Delete old files when updating a FileField
   ```
4. No migrations required – the app works automatically with existing models.  
   Test it: Delete a model instance with a `FileField` → the associated file in `MEDIA_ROOT` will be deleted.

---

## Database Management
### Django DBBackup
**Link**: [https://archmonger.github.io/django-dbbackup/5.0.0/installation/](https://archmonger.github.io/django-dbbackup/5.0.0/installation/)  
**Purpose**: Backup and restore Django databases (supports local storage, S3, Google Cloud, etc.).  

#### Full Installation & Setup
1. Install via pip:
   ```bash
   pip install django-dbbackup
   ```
2. Update `INSTALLED_APPS` in `settings.py`:
   ```python
   INSTALLED_APPS = [
       # Core apps
       'django.contrib.admin',
       # ...
       'dbbackup',  # Add this line
       # Your other apps
   ]
   ```
3. (Optional) Configure backup settings in `settings.py` (e.g., storage, compression):
   ```python
   # settings.py
   DBBACKUP_STORAGE = 'django.core.files.storage.FileSystemStorage'
   DBBACKUP_STORAGE_OPTIONS = {
       'location': os.path.join(BASE_DIR, 'backups'),  # Backup directory
   }
   DBBACKUP_COMPRESSION = 'gzip'  # Compress backups with GZIP
   DBBACKUP_DATE_FORMAT = '%Y-%m-%d_%H-%M-%S'  # Backup filename format
   ```
4. Create the backup directory (if using filesystem storage):
   ```bash
   mkdir backups
   ```
5. Basic usage (backup/restore commands):
   ```bash
   # Create a full database backup
   python manage.py dbbackup

   # Create a backup of media files (optional)
   python manage.py mediabackup

   # Restore from a backup (replace with your backup filename)
   python manage.py dbrestore --backup-file my_db_backup_2025-12-16_10-00-00.gz

   # List all backups
   python manage.py listbackups
   ```
6. (Optional) Install cloud storage backends (e.g., AWS S3):
   ```bash
   # For AWS S3
   pip install boto3
   ```
   Update settings for S3 storage:
   ```python
   # settings.py
   DBBACKUP_STORAGE = 'storages.backends.s3boto3.S3Boto3Storage'
   DBBACKUP_STORAGE_OPTIONS = {
       'access_key': os.getenv('AWS_ACCESS_KEY_ID'),
       'secret_key': os.getenv('AWS_SECRET_ACCESS_KEY'),
       'bucket_name': 'my-django-backups',
       'region_name': 'us-east-1',
   }
   ```

---

## Additional Resources
### Comprehensive Django Guide
**Link**: [https://tanvir-softwares-engineer.gitbook.io/django/](https://tanvir-softwares-engineer.gitbook.io/django/)  
**Purpose**: A detailed, beginner-to-advanced guide for Django development (covers core concepts, best practices, and project structure).  

---

## Verify All Installations
To confirm all packages are installed correctly, run:
```bash
pip freeze | grep -E "jazzmin|django-jet-reboot|django-unfold|django-extensions|python-dotenv|black|django-hijack|django-cleanup|django-dbbackup"
```
This will list all the installed packages (verify versions match your requirements).

---

*Last Updated: December 2025*