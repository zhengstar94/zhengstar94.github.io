---
toc:
    sidebar: left
layout: post
title: Habit
date: "2024-12-18"
description: track habit
img: 
importance: 10
category: Python
giscus_comments: true
---

## Main (main.py)

```python
import requests
from datetime import datetime

pixela_endpoint = "https://pixe.la/v1/users"
USER_NAME = "zxx"
TOKEN = "dasdacxzcasdadas"
GRAPH_ID = "graph1"

# TODO 1. create new user
user_params = {
    "token": TOKEN,
    "username": USER_NAME,
    "agreeTermsOfService": "yes",
    "notMinor": "yes"
}

# response = requests.post(url=pixela_endpoint, json=user_params)
# print(response.text)

# TODO 2. add new graph
graph_endpoint = f"{pixela_endpoint}/{USER_NAME}/graphs"

graph_config = {
    "id": GRAPH_ID,
    "name": "Cycling Graph",
    "unit": "Km",
    "type": "float",
    "color": "ajisai"
}

headers = {
    "X-USER-TOKEN": TOKEN
}

# response = requests.post(url=graph_endpoint, json=graph_config, headers= headers)
# print(response.text)

# TODO 3. create new pixel
pixel_creation_endpoint= f"{pixela_endpoint}/{USER_NAME}/graphs/{GRAPH_ID}"

today = datetime.now()
temp_date = datetime(year=2024, month=12, day=20)

pixel_data = {
    "date": today.strftime("%Y%m%d"),
    "quantity": input("How many kilometers did you cycle today?"),
}

# response = requests.post(url=pixel_creation_endpoint, json=pixel_data, headers= headers)
# print(response.text)


# TODO 4. update pixel
update_endpoint = f"{pixela_endpoint}/{USER_NAME}/graphs/{GRAPH_ID}/{temp_date.strftime('%Y%m%d')}"

new_pixel_data = {
    "quantity": "224.2"
}

# response = requests.put(url=update_endpoint, json=new_pixel_data, headers= headers)
# print(response.text)

# TODO 5. delete pixel

delete_endpoint = f"{pixela_endpoint}/{USER_NAME}/graphs/{GRAPH_ID}/{temp_date.strftime('%Y%m%d')}"

response = requests.delete(url=update_endpoint, headers= headers)
print(response.text)

```


