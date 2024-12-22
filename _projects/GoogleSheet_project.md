---
toc:
    sidebar: left
layout: post
title: Sheet
date: "2024-12-13"
description: Google Sheet
importance: 11
category: Python
giscus_comments: true
---

## Main

```python
from datetime import datetime
import os
import requests

APP_ID = os.environ["X_APP_ID"]
API_KEY = os.environ["X_APP_KEY"]

exercise_endpoint= "https://trackapi.nutritionix.com/v2/natural/exercise"
sheet_endpoint = os.environ["SHEET_ENDPOINT"]


TOKEN = os.environ["SHEETY_TOKEN"]

QUERY = input("input your exercise")
GENDER = "Male"
WEIGHT_KG = "75"
HEIGHT_CM = "170"
AGE = 30

headers = {
    "x-app-id": APP_ID,
    "x-app-key": API_KEY
}

parameter = {
    "query": QUERY ,
    "gender": GENDER,
    "weight_kg": WEIGHT_KG,
    "height_cm": HEIGHT_CM,
    "age": AGE
}

response = requests.post(url=exercise_endpoint, json=parameter, headers=headers)
result = response.json()

today_date = datetime.now().strftime("%d/%m/%Y")
now_time = datetime.now().strftime("%X")

for exercise in result["exercises"]:
    sheet_inputs = {
        "workout": {
            "date": today_date,
            "time": now_time,
            "exercise": exercise["name"].title(),
            "duration": exercise["duration_min"],
            "calories": exercise["nf_calories"]
        }
    }

    headers = {
        "Authorization": f"Bearer {TOKEN}"
    }

    sheet_response = requests.post(sheet_endpoint, json=sheet_inputs, headers=headers)
    print(sheet_response.text)

```

