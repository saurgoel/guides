## NLP Libraries
NLTK, TextBlob, SpaCY, Scipy Bundle, Pandas

Installing nltk	
> > > import nltk
> > > nltk.download()

###### Searching Text
There are many ways to examine the context of a text apart from simply reading it. A concordance view shows us every occurrence of a given word, together with some context. Here we look up the word monstrous in Moby Dick by entering text1 followed by a period, then the term concordance, and then placing "monstrous" in parentheses. The first time you use a concordance on a particular text, it takes a few extra seconds to build an index so that subsequent searches are fast.
```
>>> text1.concordance("monstrous")
Displaying 11 of 11 matches:
ong the former , one was of a most monstrous size . ... This came towards us ,
ON OF THE PSALMS . " Touching that monstrous bulk of the whale or ork we have r
ll over with a heathenish array of monstrous clubs and spears . Some were thick
d as you gazed , and wondered what monstrous cannibal and savage could ever hav
that has survived the flood ; most monstrous and most mountainous ! That Himmal
they might scout at Moby Dick as a monstrous fable , or still worse and more de
th of Radney .'" CHAPTER 55 Of the monstrous Pictures of Whales . I shall ere l
ing Scenes . In connexion with the monstrous pictures of whales , I am strongly
ere to enter upon those still more monstrous stories of them which are to be fo
ght have been rummaged out of this monstrous cabinet there is no telling . But
of Whale - Bones ; for Whales of a monstrous size are oftentimes cast up dead u
```

## Parsing libs
BeautifulSoup, Scrapy

## Gathering and Cleaning Data
with open("some.txt", "r") as f:
	f.readlines()

import requests
url = "some.com/dsaf"
raw_text = requests.get(url).text

## Tokenizing
Breaking up sentences into meaningful chunks.


## Stemmers and Lammetizers
Reduce words to their normalized form.
am --> be
cars --> car
```
from nltk.stem.porter import PorterStemmer
porter_stemmer = PorterStemmer()
stemmed_tokens = [porter_stemmer.stem(token) for token in tokens]
print(stemmed_tokens)
```

Stemmers are faster. They do not do much analysis as compared to lammetizers.
```
from nltk.stem import WordNetLemmatizer
wordnet_lemmatizer = WordNetLemmatizer()
lemmatized_tokens = [wordnet_lemmatizer.lemmatize(token) for token in tokens]
print(lemmatized_tokens)
```

alice = nltk.corpus.gutenberg.words("some.txt")
Frequency Distribution
But mostly the top 25 keywords it spits out are mostly punctuations and stop words.
```
from nltk import FreqDist
freq_dist_alice = FreqDist(alice)
top_25 = freq_dist_alice.most_common(25)
```

As an improvement we can look out for longer words with most frequency
```
for w in set(alice):
	if (len(w)) > 6 and freq_dist_alice[w] > 7:
		new_chart.add(w, freq_dist_alice[w])
```
	
## Part of Speech tagging
Pronoun - A pronoun is a part of a speech which functions as a replacement for a noun. Some examples of pronouns are: I, it, he, she, mine, his, hers, we, they, theirs, and ours.
Janice is a very stubborn child. She just stared at me and when I told her to stop.
The largest slice is mine.
We are number one

Adjective
This part of  a speech is used to describe a noun or a pronoun. Adjectives can specify the quality, the size, and the number of nouns or pronouns.
The carvings are intricate
I have two hamsters.

PRP - Prepositions
NN - Proper Noun
VB - Verbs

```
text = word_tokenize("this is some setene")
nltk.pos_tag(text)
```
Frequency distribution of the types of tags
```
alice_tagged = nltk.post_tag(text)
fd = FreqDist(alice_tagged)
fd_tagged = FreqDist(tag for (word, tag) in alice_tagged)
fd_tagged.most_common(10)
```

Sentiment Projects
## Rule Based Sentiment Analysis

Modifiers - things like not
incrementers - Very
decrementers - Bad

## Naive Bayes Sentiment Analysis
It works by using a training data.


###### Get things done
spaCy is designed to help you do real work — to build real products, or gather real insights. The library respects your time, and tries to avoid wasting it. It's easy to install, and its API is simple and productive. I like to think of spaCy as the Ruby on Rails of Natural Language Processing

###### Deep Learning
spaCy is the best way to prepare text for deep learning. It interoperates seamlessly with TensorFlow, Keras, Scikit-Learn, Gensim and the rest of Python's awesome AI ecosystem. spaCy helps you connect the statistical models trained by these libraries to the rest of your application.

# Spacy
# ========================================
Install: 
```
pip install spacy && python -m spacy.en.download
import spacy
```

Load English tokenizer, tagger, parser, NER and word vectors
```
nlp = spacy.load('en')
```

Process a document, of any size
```
text = open('war_and_peace.txt').read()
doc = nlp(text)
```

Hook in your own deep learning models
```
similarity_model = load_my_neural_network()
def install_similarity(doc):
    doc.user_hooks['similarity'] = similarity_model
nlp.pipeline.append(install_similarity)

doc1 = nlp(u'the fries were gross')
doc2 = nlp(u'worst fries ever')
doc1.similarity(doc2)
```