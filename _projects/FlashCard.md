---
toc:
    sidebar: left
layout: post
title: Flashy
date: "2024-12-17"
description: Flash Card
img: assets/img/2024/python/flashCard/flashy.png
importance: 6
category: Python
giscus_comments: true
---

## Main

```python
import random
from tkinter import *
import pandas


BACKGROUND_COLOR = "#B1DDC6"
current_card = {}
to_learn = {}

try:
    data = pandas.read_csv("data/word_to_learn.csv")
except FileNotFoundError:
    original_data = pandas.read_csv("data/french_words.csv")
    to_learn = original_data.to_dict(orient="records")
else:
    to_learn = data.to_dict(orient="records")



def next_card():
    global current_card, flip_timer
    window.after_cancel(flip_timer)
    current_card = random.choice(to_learn)
    canvas.itemconfig(card_title, text="French" ,fill="black")
    canvas.itemconfig(card_word, text=current_card["French"],fill="black")
    canvas.itemconfig(card_background, image=card_front_img)
    flip_timer = window.after(3000, func=flip_card)

def flip_card():
    canvas.itemconfig(card_title, text="English", fill="white")
    canvas.itemconfig(card_word, text=current_card["English"], fill="white")
    canvas.itemconfig(card_background, image=card_back_img)

def is_known():
    to_learn.remove(current_card)
    data = pandas.DataFrame(to_learn)
    data.to_csv("word_to_learn.csv", index=False)
    next_card()


window = Tk()
window.title("Flashy")
window.config(padx=50, pady=50, bg=BACKGROUND_COLOR)

flip_timer = window.after(3000, func=flip_card)

canvas = Canvas(width=800, height=526)
card_front_img = PhotoImage(file="images/card_front.png")
card_back_img = PhotoImage(file="images/card_back.png")
card_background = canvas.create_image(400, 263, image=card_front_img)
card_title = canvas.create_text(400, 150, text="", font=("Ariel",40, "italic"))
card_word = canvas.create_text(400, 263, text="", font=("Ariel",60, "bold"))
canvas.config(bg=BACKGROUND_COLOR, highlightthickness=0)
canvas.grid(row=0, column=0, columnspan=2)

cross_image = PhotoImage(file="images/wrong.png")
unknown_button = Button(image=cross_image, highlightthickness=0, command=next_card)
unknown_button.grid(row=1, column=0)

check_image = PhotoImage(file="images/right.png")
known_button = Button(image=check_image, highlightthickness=0, command=is_known)
known_button.grid(row=1, column=1)



next_card()


window.mainloop()

  
```

## Example CSV

```csv
French,English
partie,part
histoire,history
chercher,search
seulement,only
police,police
pensais,thought
aide,help
demande,request
genre,kind
mois,month
frère,brother
laisser,let
car,because
mettre,to put
aucun,no
laisse,leash
eux,them
ville,city
chaque,each
parlé,speak
arrivé,come
devrait,should
bébé,baby
longtemps,long time
heures,hours
vont,will
pendant,while
revoir,meet again
aucune,any
place,square
parle,speak
compris,understood
savais,knew
étaient,were
attention,Warning
voici,here is
pourrais,could
affaire,case
donner,give
type,type
leurs,their
donné,given
train,train
corps,body
endroit,place
yeux,eyes
façon,way
écoute,listen
dont,whose
trouve,find
premier,first
perdu,lost
main,hand
première,first
côté,side
pouvoir,power
vieux,old
sois,be
tiens,here
matin,morning
tellement,so much
enfant,child
point,point
venu,came
suite,after
pardon,sorry
venez,come
devant,in front of
vers,towards
minutes,minutes
demandé,request
chambre,bedroom
mis,placed
belle,beautiful
droit,law
aimerais,would like to
aujourd'hui,today
mari,husband
cause,cause
enfin,finally
espère,hope
eau,water
attendez,Wait
parti,left
nouvelle,new
boulot,job
arrêter,Stop
dirait,would say
terre,Earth
compte,account
donne,given
loin,far
fin,end
croire,believe
chérie,sweetheart
gros,large
plutôt,rather
aura,will have
filles,girls
jouer,to play
bureau,office
```

{% include figure.liquid loading="eager" path="assets/img/2024/python/flashCard/card_back.png" class="img-fluid rounded z-depth-1" zoomable=true width="50%"%}
{% include figure.liquid loading="eager" path="assets/img/2024/python/flashCard/card_front.png" class="img-fluid rounded z-depth-1" zoomable=true width="50%"%}
{% include figure.liquid loading="eager" path="assets/img/2024/python/flashCard/right.png" class="img-fluid rounded z-depth-1" zoomable=true width="50%"%}
{% include figure.liquid loading="eager" path="assets/img/2024/python/flashCard/wrong.png" class="img-fluid rounded z-depth-1" zoomable=true width="50%"%}
