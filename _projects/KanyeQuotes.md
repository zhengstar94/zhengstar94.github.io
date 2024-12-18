---
toc:
    sidebar: left
layout: post
title: Kanye Quotes
date: "2024-12-18"
description: Kanye Quotes
img: assets/img/2024/python/kanye/kanyeQuote.png
importance: 8
category: Python
giscus_comments: true
---

## Main

```python
from tkinter import *
import requests


def get_quote():
    response = requests.get("https://api.kanye.rest/")
    response.raise_for_status()
    data = response.json()
    quote = data["quote"]
    canvas.itemconfig(quote_text, text=quote)



window = Tk()
window.title("Kanye Says...")
window.config(padx=50, pady=50)

canvas = Canvas(width=300, height=414)
background_img = PhotoImage(file="background.png")
canvas.create_image(150, 207, image=background_img)
quote_text = canvas.create_text(150, 207, text="Kanye Quote Goes HERE", width=250, font=("Arial", 30, "bold"), fill="white")
canvas.grid(row=0, column=0)

kanye_img = PhotoImage(file="kanye.png")
kanye_button = Button(image=kanye_img, highlightthickness=0, command=get_quote)
kanye_button.grid(row=1, column=0)



window.mainloop()
        
```

{% include figure.liquid loading="eager" path="assets/img/2024/python/kanye/background.png" class="img-fluid rounded z-depth-1" zoomable=true width="50%"%}
{% include figure.liquid loading="eager" path="assets/img/2024/python/kanye/kanye.png" class="img-fluid rounded z-depth-1" zoomable=true width="50%"%}

