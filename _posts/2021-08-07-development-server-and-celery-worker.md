---
layout: default
title: How to develop FastAPI apps using celery efficiently
excerpt: FastAPI is a new and very popular framework for developing python web apis. Instead of using the two terminals and commands, we can use Celerys testing utilities to start the celery worker in a background thread.
tags: [python, FastAPI, celery]
---

# FastAPI and Celery

FastAPI is a new and very popular framework for developing python web apis.
Celery is probably the most used python library for running long running tasks within
web applications.
FastAPI and Celery are often used together (the FastAPI documentation even recommends this) and applications like data science and machine learning, where longer running cpu bound tasks need to be completed asynchronously are an ideal match for the combination of libraries.

# Developing

To demonstrate a simple application we'll start with a fresh python virtual environment and install our libraries (we're assuming that redis is installed started on localhost here and you've created a virtual environment with python 3.9 or similar):

```
pip install fastapi celery[redis] uvicorn
```

Now let's create a few files. The first file (`tasks.py`) defined our celery tasks, while the second file (`main.py`) defines a very simple FastAPI app that uses one of the celery tasks.

<script src="https://gist.github.com/daviskirk/e2f885288f3108aa4b17cee41bd8c883.js?file=tasks.py"></script>
<script src="https://gist.github.com/daviskirk/e2f885288f3108aa4b17cee41bd8c883.js?file=main.py"></script>

When developing such applications, developers typically start two processes in separate terminals:

1. The api server (running the FastAPI app) and
2. The celery worker (running the celery tasks)

While having these processes separate is critical in production, during development it most often isn't an issue to have these running in the same process.
Running both in the same process allows a simpler development flow, since we only need one command and one terminal to start developing.

# Solution

Instead of using the two terminals and commands, we can use Celerys testing utilities to start the celery worker in a background thread (note that because of the way python's threads work, CPU bound tasks will block the API server, which obviously is not good for deployment, but for development doesn't bother us in many cases and is much nearer to reality than using the `task_always_eager` setting).

To do that, we're going to wrap the web servers (we're using Uvicorn here) command:

<script src="https://gist.github.com/daviskirk/e2f885288f3108aa4b17cee41bd8c883.js?file=run.py"></script>

This might look strange because we're using a few hacks here but the important part is the `celery.contrib.testing.worker.start_worker` to start the celery worker in the background. The other code is a hack to wrap the Uvicorn cli command so that we can use our new script the same way we would otherwise use Uvicorn. `python run.py` will accept the same commands as the `uvicorn` command:

```
python run.py --help
```

Now we can just start our web server almost the same way that we would otherwise:

```
python run.py main:app --reload
```

but instead of just starting the web server, it will also start a celery worker in the background.

Visiting `http://127.0.0.1:8000` will now show:

```
{"2+2":4}
```

with the result being calculated in the celery task!
