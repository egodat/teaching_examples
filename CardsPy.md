---
jupyter:
  jupytext:
    formats: ipynb,md
    text_representation:
      extension: .md
      format_name: markdown
      format_version: '1.3'
      jupytext_version: 1.13.8
  kernelspec:
    display_name: Python 3 (ipykernel)
    language: python
    name: python3
---

# Cards and Python


## The basics

```python
import random
```

First we make a list of cards. This is for a single suit, we could do a full deck if we wanted

```python
cards = ['A','2','3','4','5','6','7','8','9','10','J','Q','K']
```

Then use `randint` to generate indicies for our list instead of the actual cards

```python
cards[random.randint(0,len(cards)-1)] #This line effectively draws a card at random (with replacement)
```

we used `len(cards)` so it is more flexible if we add more cards.
The -1 is to correct for mismatch between length and our indicies `len(0,...,12) = 13`


## Testing the Randomness


Here we test the random card selector 10 times

```python
for el in range(0,10):
    print(cards[random.randint(0,len(cards)-1)])
```

Then since 10 seemed too few, we can do 1 million mock draws

```python
trials=[]
for el in range(0,1000000):
    trials.append(cards[random.randint(0,len(cards)-1)])
```

```python
import pandas as pd #Importing pandas for some analytics and graphing
```

We can convert the list of 1 million trials to a pandas series so that we can use values counts. This counts each time the value (card) appears in our series (list of trials)

```python
vc=pd.Series(trials).value_counts()
vc
```

We can then plot the values from our value counts. If our cards are in fact "random", we should get essentially equal bars

```python
vc.plot(kind='bar')
```

Notice that if we were to run this code again, we'd get a different order and values due to the randomness.


## Math with cards


If we want to do math with the cards, we can build a dictionary that maps the card name to the card value.
This is safer and more flexible than using the card names themselves.
E.g. An Ace might have different values in different games.

```python
cd = {'A':1,'2':2,'3':3,'4':4,'5':5,'6':6,'7':7,'8':8,'9':9,'10':10,'J':11,'Q':12,'K':13}
```

```python
cd[cards[random.randint(0,len(cards)-1)]]
```

Using the dictionary, we can draw multiple cards and then do math with them.

```python
c1 = cards[random.randint(0,len(cards)-1)] # These are card names as strings
c2 = cards[random.randint(0,len(cards)-1)]

result = cd[c1]+cd[c2]  # We can then use the names as keys in our dictionary to get values

print("{} + {} = {}".format(c1,c2,result))
```

## Building a Deck


Here we can build out a deck much the same way we built the initial list of cards. This uses nested loops to do all suits and all cards

```python
suits = ["_H","_C", "_D", "_S"] # I like the underscore here for readability and for a cool trick later
deck = [] # Initial empty deck
for el in suits: # Loop over suits
    for card in cards: # Loop over cards
        deck.append(card + el) #Add the cards into our deck
deck
```

## Drawing through a deck


Here we write a function to keep us from having to write the same code over and over when we want to draw a card

```python
def draw_card(cards): #Takes in a list of cards
    return cards[random.randint(0,len(cards)-1)] # Returns a card at random from our list 
```

We can test our function

```python
draw_card(deck)
```

Here we loop over the deck and remove the cards we draw, if we did this correctly, every card will get drawn in random order.
Keep in mind, this is effectively shuffling the deck after each draw.
This shouldn't change anything as we have no knowledge of the order of the deck after we draw but is worth mentioning.

```python
test_deck = deck.copy()
# We can force python to make a copy of our data
#This is only needed if we don't want to change the original deck


for el in range(0,len(deck)): #we want to use the length of the original deck here since our test_deck will get smaller as we draw cards. A while loop could be better here.
    draw = draw_card(test_deck)
    print("We draw {} from {} remaining cards".format(draw,len(test_deck)))
    test_deck.remove(draw)
```

```python
test_deck = deck.copy()

while len(test_deck) > 0: # Same as above but with while instead
    draw = draw_card(test_deck)
    print("We draw {} from {} remaining cards".format(draw,len(test_deck)))
    test_deck.remove(draw)
```

## Doing Math with a Full Deck


The last thing we can do is do math with our entire deck. Since we used the underscore in our card names and the dictionary to assign our card values, we can combine those to not have to add anything to our dictionary to get value for any card in the deck.

```python
test_deck2 = deck.copy()

draw1 = draw_card(test_deck2)
draw2 = draw_card(test_deck2)

total = cd[draw1.split('_')[0]] + cd[draw2.split('_')[0]]
# Here we are splitting our card names ('A_S') into name ('A') and suit ('S').
#Then we can take the 0th element, the name ('A'), as the key to our dictionary to get the value (1)

print("{} + {} = {}".format(draw1, draw2, total))
```

If we want to draw a card and then draw from the deck again (without replacement), we just need to remove the card we draw from the deck.

```python
test_deck2 = deck.copy()

draw1 = draw_card(test_deck2)

test_deck2.remove(draw1)

draw2 = draw_card(test_deck2)

result = cd[draw1.split('_')[0]] + cd[draw2.split('_')[0]]

print("{} + {} = {}".format(draw1, draw2, result))
```

```python

```
