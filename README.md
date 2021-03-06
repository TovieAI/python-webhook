## Python webhook template

Please read more about Tovie DialogStudio webhooks in our [Help Center](https://help.aimylogic.com/en/article/webhook-14yx2uz/).

## How to run on Heroku
[heroku.com](http://heroku.com) provides a free plan for Python applications.
Please click on the button below to run your copy of this webhook on Heroku

[![Deploy](https://www.herokucdn.com/deploy/button.svg)](https://heroku.com/deploy?template=https://github.com/TovieAI/python-webhook)

Heroku will build and deploy your webhook automatically. After that you have to provide a public URL of your webhook in your bot's settings.

### How to upload code changes
Please make the next steps to upload your changes on Heroku.

Install [git](https://git-scm.com/downloads) and [Heroku CLI](https://devcenter.heroku.com/articles/heroku-cli#download-and-install).
Run a terminal (or console) on your machine and type

```
heroku login
heroku git:clone -a <your Heroku application name>
cd <your Heroku application name>
git remote add origin https://github.com/TovieAI/python-webhook
git pull origin master
```

_You have to do these steps only once._

Once you are ready to upload your changes to Heroku, please type

```
git add .
git commit -am "some comments"
git push
```

Heroku will build and deploy your changes automatically.

## How to run this template locally in development mode
Run a terminal (or console) and jump into the folder with your copy of this template.
Type `python3 setup.py develop`, then `runwebhook`.

Webhook will be running at localhost `0.0.0.0:5000`. To let Tovie DialogStudio server send requests to your webhook, you have to share localhost url `0.0.0.0:5000` with, e.g., [ngrok](https://ngrok.com/). Download ngrok and run it with:
* ngrok.exe http 5000 _(windows)_
* ./ngrok http 5000

Copy it and paste into the field named "Webhook for tests" in your bot's settings.
**All requests to your webhook will go to your local machine** while you test your bot scenario via a test widget on Tovie DialogStudio. This is very useful for rapid development and debugging purposes.

_Note that you don't have to restart the local server each time you change any source file. Flask will handle it for you serving the same public URL._

## How to use this template
`webhook.py` file contains all source code you need to change.
Here you can add/remove action handlers for your webhook (please read more about webhook actions on [Help Center](https://help.aimylogic.com/en/article/webhook-14yx2uz/)).

### How to add action handler
You can change action behaviour in _webhook_ function inside `webhook.py`. 

*webhook.py*
```python
def webhook(session):
    action = session['action']

    if action == 'event1':
        print('Received request from event1 action')
        session['variable'] = 'some value'

    return json.dumps(session)
```

Once the bot steps into the screen with enabled action _action1_, it calls your webhook and receives variable named _variable_ with value _"some value"_.

### How to return multiple variables
Just add variables in _session_ object

```python
def webhook(session):
    action = session['action']

    if action == 'event1':
        print('Received request from event1 action')
        session['variable1'] = 'value1'
        session['variable2'] = 'value2'

    return json.dumps(session)
```

This will provide _variable1_ and _variable2_ in Tovie DialogStudio scenario.

### How to handle multiple actions
You can add as many handlers as you need:


*webhook.py*
```python
def webhook(session):
    action = session['action']

    if action == 'event1':
        print('Received request from event1 action')
        session['variable1'] = "value1"

    elif action == 'event2':
        print('Received request from event2 action')
        session['variable2'] = "value2"

    return json.dumps(session), 200
```
