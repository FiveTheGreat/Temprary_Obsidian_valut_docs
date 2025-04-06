Awesome! Since you have Django installed, let's start by setting up the project.

### **Step 1: Create a Django Project**

Run the following command to create your event booking project:

```bash
django-admin startproject event_booking
cd event_booking
```

### **Step 2: Create Django Apps**

Django follows the "app" structure, so we'll create separate apps for different functionalities:

```bash
python manage.py startapp users    # Handles authentication
python manage.py startapp events   # Manages event listings
python manage.py startapp bookings # Handles event bookings
```

### **Step 3: Register Apps in `settings.py`**

Open `event_booking/settings.py` and add these apps under `INSTALLED_APPS`:

```python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'users',
    'events',
    'bookings',
]
```

### **Step 4: Run Initial Migrations**

Before proceeding, apply migrations to set up the default database:

```bash
python manage.py migrate
```

This sets up Django’s built-in authentication system and database structure.

---

Why we DO?
1. **Created a Django Project (`startproject`)**
    - This sets up the basic folder structure and configurations for a Django application.
    - The `event_booking` folder contains settings, URLs, and WSGI/ASGI configurations.
2. **Created Apps (`startapp`)**
    
    - Django follows a modular structure where different features are managed in separate apps.
    - **`users` app** → Manages authentication and user profiles.
    - **`events` app** → Handles event creation, listing, and details.
    - **`bookings` app** → Manages user bookings for events.
    - Keeping things modular makes the project **scalable and maintainable**.
3. **Registered Apps in `settings.py`**
    
    - Django doesn’t automatically detect new apps.
    - We add them in `INSTALLED_APPS` so Django recognizes them and applies migrations when needed.
4. **Ran Migrations (`migrate`)**
    
    - Django uses a built-in ORM (Object-Relational Mapper) to interact with the database.
    - Running `migrate` applies default database tables (like `auth_user`) and ensures everything is set up properly.