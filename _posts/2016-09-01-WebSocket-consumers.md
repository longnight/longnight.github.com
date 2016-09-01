---
published: true
layout: post
---
WebSocket consumers

https://blog.heroku.com/in_deep_with_django_channels_the_future_of_real_time_apps_in_django#websocket-consumers

Now, requests should go through. This illustrates something pretty neat: Channels continues to handle HTTP(S) requests just fine, but it does it in a complete different way. this isn’t too different from running Celery with Django, where you’d run a Celery worker alongside a WSGI server. Now, though, all tasks — HTTP requests, WebSockets, background tasks — get run in the worker.
(By the way, we can still run python manage.py runserver for easy local testing. When we do, Channels simply runs Daphne and a worker in the same process.)