First off, thanks so much for having us do this homework. I really enjoyed working
on it. Probably way too much. I was supposed to turn in it last night when I got
97% accuracy on the dev set, but I kept going and got `99%`. Part of me still wants
to explore more features and get the worlds highest. I'll probably do it later. But
yeah, it was a nice homework!

### Experiment 1
When I first look at the problem set, it somehow intuitively made sense to go with
SVM(since SVMs are really good discriminative models). I started off by picking
the features which first crossed my mind.


##### Feature 1: length of L
```
I picked this because, the length L tends to be shorter for cases when there is
an acronym(U.S,Mr.,Dr.,).
```

##### Feature 2: is R uppercase
```
Most of the sentence boundaries end with a punctuation and start with
an uppercase. So I thought this would be an important feature to have.
```

##### Feature 3: L contains period
```
I was checking to see if L word has periods in it like U.S etc. I did
this because, it might help the model to be "aware" of such tricky
situations. I'm not quite sure if it helped the model, but I would be
interested to individually remove one of these features one at a time and
see how they impacted the model.
```

##### Feature 4: POS of R
```
I have observed that most of the sentences start with a Noun or Adverbs
like The.
```

##### Feature 5: POS of L
```
With a similar intuition like 4, I thought it might help to get the
POS of the left word and have it as a feature
```

I then stumbled for quite a bit on how to encode some of these features which
had "labels" as their values. I figured one hot encoding could help me do that.
But then I was facing an other problem where some of the feature type labels
where not observed in dev/test data. Upon some pondering and discussions, I
decided to combine my test/train/dev split while doing the one hot encoding and
then split them later accordingly before actual training. So with those features
in hand I ended up getting a whooping `97%` accuracy which is better than the
NLTK's punkt. Here are the full results:

```python
                  precision    recall  f1-score   support

False              0.93         0.95      0.94      1766
True               0.99         0.98      0.98      5769

avg / total        0.97         0.97      0.97      7535
```


##### Evaluation & Error analysis
I noticed a common error where the model couldn't split the sentence which was
ending with a punctuation like %. Here is an example which my model got wrong
`"which it recently bought 49%. It said that volume makes it the" has been
classified as  False and should be True`. I guess this might be because I'm
taking the length of L into consideration.

Something else which I noticed my model was missing was the roman alphabet. For
instance it couldn't split the following correctly `a principal of the firm,
Henry I. Otero of Miami, were jointly fin`.

Another common error I noticed where my model was failing was the U.S. cases
(of course) . I tried coming up with a feature which can add some weight to L
words which have periods in them earlier, but that seemed not work very well.  


### Experiment 2

For this experiment I used a "rbf" kernel for SVM to smooth boundaries instead
of having linear boundaries like my earlier model. I also added new features
which were partly based off the faults I found with my first model.

##### Feature 6: Left raw has verb
```
I got this idea from a paper which you can find here
http://honors.cs.umd.edu/reports/Nilani.pdf
This paper neatly outlines a logic about sentence structure.
The author claims that pretty much every sentence has a verb before it ends.
With this in mind, I'm adding two features which check if there is a verb
before or after the punctuation
```

##### Feature 7: Right raw has verb


##### Feature 8: Is left Punctuation
```
I added this feature to fix the punctuation before period error which I found
with my previous model.
```

##### Feature 9: Is right number
```
After noticing the punctuation before punctuation(period) error, I added
the feature 8 to take that into consideration. But then I realized I might miss
on sentences which might start with a number like for instance `I sold it for a
profit of $.99 and made` In this case `$` will be treated a punctuation, but it
clearly is not a sentence boundary.

```

##### Feature 10: is L uppercase
```
There are some sentences which have roman alphabet which are uppercase english
letters. So I thought I should add it a feature so that the model could gauge
if it's a roman alphabet or not by looking at the length feature weight.
```

##### Evaluation & Error analysis

Although I got a high accuracy, my model still fails on U.S. cases.
`"ares with estimates that the U.S. derivatives market is perhaps"  classified
as  True should be  False`

 It also does fail on some cases of Mr. and Mrs. But I did get rid if the
 punctuation before punctuation confusion.
 `"will join die-hard fans like Mr. de Castro and pack the stands to"
 classified as True should be  False`

 I don't see any sentences which end with `%` being misclassified. I think one
 could improve this model further by having a trigram and bigram probabilities
 of L with punctuation and R with punctuation as features.

 Adding these features and tinkering with the kernel and hyper parameters gave me a
 nice `99%` overall accuracy! Here are the full results:

```python
              precision     recall    f1-score

False         0.99          0.97      0.98
True          0.99          1.00      0.99

avg/total     0.99          0.99      0.99
```
