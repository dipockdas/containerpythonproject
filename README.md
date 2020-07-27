# Simple Container project

Python sucks ! Well the language is OK but the damn environment mess is a disaster. Try to use multiple projects on your machine lands you in hell. 

The solution - containerize your projects. The docker lab example is fine but there's a few bugs - so this is the simple guide with fixes and gotchas.


## The app

Create the following directory structure with two files, requirements.txt and server.py.

Note: the docker website code example had a couple of issues. The code sample had a misplaced indent and the code returned a null response. This is fixed in the example below. 


```
app
├─── requirements.txt
└─── src
     └─── server.py
```

In server.py add the following

```
from flask import Flask
import os
server = Flask(__name__)

@server.route("/")
def hello():
    return "Hello World!"

if __name__ == "__main__":
   port = int(os.environ.get("PORT", 5000))
   server.run(debug=True,host='0.0.0.0',port=port)
   
```

In the requirements.txt file add the dependencies

```
Flask==1.1.1
```

In the app root directory create a Dockerfile and add the following instructions.

```
# set base image (host OS)
FROM python:3.8

# set the working directory in the container
WORKDIR /code

# copy the dependencies file to the working directory
COPY requirements.txt .

# install dependencies
RUN pip install -r requirements.txt

# copy the content of the local src directory to the working directory
COPY src/ .

# command to run on container start
CMD [ "python", "./server.py" ]
```

Now build the docker image

```
docker build -t myimage .
```

List the images

```
docker images
```

Run the image

```
docker run -d -p 5000:5000 myimage
```

Check the container is running

```
docker ps -a
```

Check the logs if the container fails
```
docker logs <container_name>
```

If the container is running - test the app using cURL

```
curl http://localhost:5000
```









 
