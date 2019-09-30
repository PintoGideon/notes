Django was invented to meet fast moving newsroom deadlines while satisfying the tough requirements of experienced Web Developers

```python
python3 -m venv name_of_local_environment
django-admin startproject mysite .
ls
local_env manage.py mysite quotes
python3 manage.py migrate
python3 manage.py runserver


```

2. Configuration

- Set SECRET_KEY and DEBUG in name_of_local_environment/bin/activate
- Add quotes app to INSTALLED_APPS
- Add mysite/template to TEMPLATES/DIRS
