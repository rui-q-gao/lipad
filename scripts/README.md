## The First Script - hansard_corpus.ipynb
### In this script, the collected lipad dataframe pkl will be processed, into a corpus of text. 
The reason why we go through the intermediate step of translating to a corpus is so we can reduce the memory usage and segment the process. More explanation in the second script.

Here are the different things to play/tune around here:

1. Choice of standardizing defunct/merged party names, described below
2. Choice of stopwords
3. Choice of stemming/lemmatization (none at the moment)
4. Choice of replacement words eg. abortion, immigration. It's here instead of downstream because replacement is dependent on date and party information, which is ultimately abandoned in the collected corpus. 


## The Second Script - hansard_training.ipynb
### In this script, the corpus will be streamed into a gensim Word2Vec model and the model embeddings will be trained. 

Here are the different things to play/tune around here:

1. Custom corpus object and final processing step. Wondering why there's a weird corpus object? Hint: it's **memory**.  Check [here](https://radimrehurek.com/gensim/auto_examples/core/run_corpora_and_vector_spaces.html#corpus-streaming-tutorial)! 
2. All the hyperparameters for the model! Dimensionality, minimum word count, number of worker threads, context window, and downsampling can all be changed here. Number of epochs isn't a huge deal, check out this [Stack Overflow answer](https://stackoverflow.com/a/46857922). 
3. Choice of colocation of common phrases - gensim.models.phrases.Phrases is currently used. For brevity sake, the Phrases model is not trained repeatedly, TODO - Phrases should be trained on non-replaced corpus. 

**Note**: on a 2018 MacBook Pro, the training took roughly an hour. Go and cook a nice meal, read a book, get some exercise in and let it run! 


## The Third Script - hansard_application.ipynb
### In this script, the saved model word vectors are used to do analysis.  

Here are the different things to play/tune around here:

1. I've made graphs of the pairwise similarity of abortion keywords throughout the years, for each pair of Libs, Cons, and NDP. 
2. I've made graphs of the similarity to certain keywords (contraception, murder, etc.) for each party's use of abortion keywords.


## Sandbox - sandbox.ipynb
### For data exploration and other operations not part of the central workflow

So far, there are two things being analyzed here:
1. The number of occurences by year per group of the abortion replacements. This is for quick confirmation of correctness (check that number of total occurences doesn't change by group, etc) as well as determining the maximum min_count threshold that still results enough years included (no giant leaps). I've taken the liberty to visualize these years, so that the jumps can be immediately seen. 

2. Review the number of replacements made per each sub-regex, and view the total number of replacements. It should be ~27,000. 
