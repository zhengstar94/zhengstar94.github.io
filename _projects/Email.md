---
toc:
    sidebar: left
layout: post
title: Email
date: "2024-12-18"
description: Auto send birthday email
img: assets/img/2024/python/email/email.png
importance: 7
category: Python
giscus_comments: true
---

## Main

```python
import smtplib
from datetime import datetime
import pandas
import random

MY_EMAIL = "your@email.com"
MY_PASSWORD = "yourpassword"

today = datetime.now()
today_tuple = (today.month, today.day)

data = pandas.read_csv("birthdays.csv")
birthdays_dict = {(data_row["month"], data_row["day"]): data_row for (index, data_row) in data.iterrows() }
if today_tuple in birthdays_dict:
    birthday_person = birthdays_dict[today_tuple]
    file_path = f"letter_templates/letter_{random.randint(1,3)}.txt"
    with open(file_path) as letter_file:
        contents = letter_file.read()
        contents = contents.replace("[NAME]", birthday_person["name"])

    with smtplib.SMTP("smtp.gmail.com") as connection:
        connection.starttls()
        connection.login(MY_EMAIL, MY_PASSWORD)
        connection.sendmail(from_addr=MY_EMAIL,
                            to_addrs=birthday_person["email"],
                            msg=f"Subject:Happy Birthday!!!\n\n{contents}"
                            )



        
```