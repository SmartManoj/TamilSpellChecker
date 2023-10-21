# Tamil Spell Checker

The idea for building a simple Tamil Spell Checker came from a conversation with T Shrinivasan from the open-tamil team. 

Tamil Spell Checker uses the below approach to suggest different spellings for a word

- Check whether it is a valid Tamil word using Bloom Filter
- Use Levenstein Distance (edit distance of 2) to suggest words when it is not a Tamil word 

## Project Madurai Crawler

Project Madurai has a good collection of Tamil works. Use Project Madurai Crawler to generate a Tamil unique word list. 

To run it use the below command 
```
python ProjectMaduraiCrawler.py
```

## Create Bloom Filter File 

Bloom Filter is a space-efficient and compute-optimized probabilistic data structure designed to tell whether an item is present in a set. More information on Bloom Filter can be found in [wiki](https://en.wikipedia.org/wiki/Bloom_filter).

- Spellchecker is using Bloom Filter to check whether a word is a valid Tamil word or not. 
- Bloom Filter Datastructure file has to be created first before being used to check the validity of a word 

To generate a Bloom Filter file use the below command 

```
python TamilBloomFilterCreator.py
```

## Sample code to check whether a word is a valid Tamil word

```
from tamilspellchecker.TamilwordChecker import TamilwordChecker
from tamilspellchecker.TamilSpellingAutoCorrect import get_data

unique_word_count = 2043478
tamilwordchecker = TamilwordChecker(unique_word_count,get_data("tamil_bloom_filter.txt"))
print(tamilwordchecker.tamil_word_exists("மேகம்"))
```

## Sample code to check get spell check corrections 

```
from tamilspellchecker.TamilSpellingAutoCorrect import TamilSpellingAutoCorrect, get_data
spellchecker = TamilSpellingAutoCorrect(get_data("tamil_bloom_filter.txt"), get_data("tamilwordlist.txt"))
from_spell_checker_list = spellchecker.tamil_correct_spelling("மேக்ம்")
print(from_spell_checker_list)
```

## Norvig Algorithm
Norvig algorithm can run faster than the exhaustive search method; you
can use it as follows,

```
from tamilspellchecker.TamilSpellingAutoCorrect import TamilSpellingAutoCorrect, get_data
from pprint import pprint
from tamil.utf8 import get_letters
spellchecker = TamilSpellingAutoCorrect(get_data("tamil_bloom_filter.txt"), get_data("tamilwordlist.txt"))
results = spellchecker.tamil_Norvig_correct_spelling("தமிழ்னாடு") #தமிழ்நாடு என்பது சரியான சொல்.
results = list(filter(lambda x: len(get_letters(x)) >= 4,results )) #filter for words >= 4 letters
results = list(filter(lambda x: len(get_letters(x)) <= 6,results )) #and for words <= 6 letters
pprint(results)
assert 'தமிழ்நாடு' in results
```

## Accuracy Issues

The accuracy of Tamilwordchecker depends on the list of unique words that are there in tamilwordlist.txt. We need to add more unique words from other sources. 



