
https://github.com/GoogleCloudPlatform/vertex-ai-samples/blob/main/notebooks/community/ml_ops/stage6/get_started_with_fastapi.ipynb


Fast API Server
```python
from fastapi.logger import logger
from fastapi import FastAPI, Request
import tensorflow as tf

import numpy as np
import argparse
import os

import logging

gunicorn_logger = logging.getLogger('gunicorn.error')
logger.handlers = gunicorn_logger.handlers

if __name__ != "main":
    logger.setLevel(gunicorn_logger.level)
else:
    logger.setLevel(logging.DEBUG)

app = FastAPI()

logger.info("Loading model")
model = tf.saved_model.load('/model')

@app.get(os.environ['AIP_HEALTH_ROUTE'], status_code=200)
def health():
    """ health check to ensure HTTP server is ready to handle 
        prediction requests
    """
    return {"status": "healthy"}


@app.post(os.environ['AIP_PREDICT_ROUTE'])
async def predict(request: Request):
    body = await request.json()
    instances = body["instances"]
    inputs = []
    for instance in instances:
        inputs.append(instance['bytes_inputs']['b64'])

    # unfinished, returns Internal Server error
    #outputs = model.predict(inputs)
    #logger.info(f"Outputs {outputs}")
    #return {"predictions": [class_num for class_num in np.argmax(outputs, axis=1)]}
    return {'test': 'to-be-finished'}

```
##### Performance:
FastAPI is built on Starlette and Pydantic, making it faster than Flask. It is particularly well-suited for high-performance applications due to its asynchronous capabilities.

##### Asynchronous Support: 
It natively supports asynchronous request handling, which is beneficial for IO-bound and high-concurrency applications.

##### Type Checking and Validation: 
FastAPI uses Python 3.6+ type declarations for request and response data models. This results in automatic request validation and serialization.

##### Automatic API Documentation: 
It generates documentation (using Swagger UI and ReDoc) for your APIs automatically from your Python code.

##### Modern Python Features: 
It leverages the latest Python features and is designed for Python 3.6 and above.


The learning curve might be steeper if you're not familiar with asynchronous programming.