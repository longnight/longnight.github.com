---
published: true
layout: post
---
WebSocket consumers

https://blog.heroku.com/in_deep_with_django_channels_the_future_of_real_time_apps_in_django#websocket-consumers

Now, requests should go through. This illustrates something pretty neat: Channels continues to handle HTTP(S) requests just fine, but it does it in a complete different way. this isn’t too different from running Celery with Django, where you’d run a Celery worker alongside a WSGI server. Now, though, all tasks — HTTP requests, WebSockets, background tasks — get run in the worker.
(By the way, we can still run python manage.py runserver for easy local testing. When we do, Channels simply runs Daphne and a worker in the same process.)

```
@channel_session
def ws_receive(message):
    # Look up the room from the channel session, bailing if it doesn't exist
    try:
        label = message.channel_session['room']
        room = Room.objects.get(label=label)
    except KeyError:
        log.debug('no room in channel_session')
        return
    except Room.DoesNotExist:
        log.debug('recieved message, buy room does not exist label=%s', label)
        return

    # Parse out a chat message from the content text, bailing if it doesn't
    # conform to the expected message format.
    try:
        data = json.loads(message['text'])
    except ValueError:
        log.debug("ws message isn't json text=%s", message['text'])
        return

    if set(data.keys()) != set(('handle', 'message')):
        log.debug("ws message unexpected format data=%s", data)
        return

    if data:
        log.debug(
            'chat message room=%s handle=%s message=%s',
            room.label, data['handle'], data['message'])
        m = room.messages.create(**data)

        # import pdb; pdb.set_trace()
        m
        print('we are at ws receive!!!!' + '*'*100)

        # See above for the note about Group
        Group('chat-'+label, channel_layer=message.channel_layer).send(
            {'text': json.dumps(data)})
            
```

  