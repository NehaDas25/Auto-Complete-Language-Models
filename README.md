# Work Report

## Information

- Name: <ins> DAS, NEHA </ins>
- GitHub: <ins> NehaDas25 </ins>


## Features

- Not Implemented:
  - PART 2: In Part 2 Estimate probabilities for all words and Count Probabilities and Matrices were already implemented only we have to run the code to test our graded function.

  - PART 4: In part 4, Get multiple suggestions and Suggest multiple words using n-grams of varying length were already implemented only we have to run the code to test our graded function.

<br><br>

- Implemented:
  - PART 1: LOAD AND PREPROCESS THE DATA
  - PART 1.2: PRE-PROCESS THE DATA
    - Exercise 1: split_to_sentences
      - Implemented a function split_to_sentences() with data as input.
      - Split the data into sentences using str.split and split data by linebreak "\n".
      - This passed the unit-test case.
    
    - Exercise 2: tokenize_sentences
      - Implemented a function tokenize_sentences() with sentences as input. 
      - Converted all tokens into lower case using lower() so that words which are capitalized in the original text are treated the same as the lowercase versions of the words.
      - Append each tokenized list of words(using nltk.word_tokenize) into a list of tokenized sentences.
      - This passed the unit-test case as well.
    
    - Exercise 3: get_tokenized_data
      - Implemented a function get_tokenized_data() with data as input.
      - Get the sentences by splitting up the data using split_to_sentences().
      - Get the list of lists of tokens by tokenizing the sentences using tokenize_sentences().
      - This passed the unit-test case as well.

    - Exercise 4: count_words
      - Implemented a function count_words() with tokenized_sentences as input.
      - Here we will focus onthe words that appear at least N times in the data.
      - First of all count how many times each word appears in the data.
      - Used double loop, one for sentences and the other for tokens within a sentence.
      - Returns word_counts.
      - This passed the unit-test case as well.

    - Exercise 5: get_words_with_nplus_frequency
      - Implemented a function get_words_with_nplus_frequency() with tokenized_sentences, count_threshold as inputs.
      - Any word whose count is greater than or equal to the threshold count_threshold is kept in the closed vocabulary.
      - Returns the word closed vocabulary list.
      - This passed the unit-test case as well.

    - Exercise 6: replace_oov_words_by_unk - This is implementation is for, **if our model is performing autocomplete, but encounters a word that it never saw during training, it won't have an input word to help it determine the next word to suggest. The model will not be able to predict the next word because there are no counts for the current word.** 
      - Implemented a function replace_oov_words_by_unk() with tokenized_sentences, vocabulary, unknown_token as inputs. 
      - Modified the training data so that it has some 'unknown' words to train on.
      - Words to converted into "unknown" words are those that do not occur very frequently in the training set.
      - Created a list of the most frequent words in the training set, called the closed vocabulary(the words that appear count_threshold times or more) .
      - Converted all the other words that are not part of the closed vocabulary to the token 'unk'.
      - This passed the unit-test case as well.

    - Exercise 7: preprocess_data
      - Implemented a function preprocess_data() that takes train_data, test_data, count_threshold, unknown_token, get_words_with_nplus_frequency(),replace_oov_words_by_unk() as inputs.
      - Returns train_data_replaced, test_data_replaced, vocabulary.
      - Found tokens that appear at least count_threshold times in the training data.
      - Replaced tokens that appear less than count_threshold times by "unk" both for training and test data.
      - This passed the unit-test case as well.

  - PART 2: Develop n-gram based Language Models
    - Exercise 8: count_n_grams
      - Implemented a function count_n_grams() that takes data, n, start_token, end_token as inputs.
      - Loop through each sentence in the data.
      - Prepend start token n times, and append the end token one time.
      - The key of each key-value pair in the dictionary should be a tuple of n words (and not a list).
      - Used 'i' to indicate the start of the n-gram from index 0 to the last index where the end of the n-gram is within the sentence.
      - Get the n-gram from i to i+n.
      - Checked if the n-gram is in the dictionary then increase the count for this n-gram otherwise initialized this n-gram count to 1.
      - Returns n_grams.
      - This passed the unit-test case as well.

    - Exercise 9: estimate_probability
      - Implemented a function estimate_probability() that takes word, previous_n_gram,n_gram_counts, n_plus1_gram_counts, vocabulary_size, k=1.0 as inputs.
      - Returns Probability.
      - Set the denominator,if the previous n-gram exists in the dictionary of n-gram counts and get its count.Otherwise set the count to zero and use the dictionary that has counts for n-grams.
      - Calculated the denominator using the count of the previous n gram and apply k-smoothing, *denominator = previous_n_gram_count + (k*vocabulary_size)*.
      - Defined n plus 1 gram as the previous n-gram plus the current word as a tuple.
      - Set the count to the count in the dictionary, otherwise 0 if not in the dictionary and use the dictionary that has counts for the n-gram plus current word.
      - Calculated the numerator use the count of the n-gram plus current word,and apply smoothing, *numerator = n_plus1_gram_count + k*.
      - Calculated the probability as the numerator divided by denominator.
      - This passed the unit-test case as well.

  - PART 3: Perplexity
    - Exercise 10: calculate_perplexity
      - Implemented a function calculate_perplexity() that takes sentence, n_gram_counts, n_plus1_gram_counts, vocabulary_size, start_token, end_token, k=1.0 as inputs.
      - Find the length of previous words and store it in n. 
      - Prepend start_token and append end_token.
      - Cast the sentence from a list to a tuple.
      - Calculated the length of sentence (after adding start_token, end_token) and store under N.
      - The variable p will hold the product that is calculated inside the n-root that is *product_pi = 1.0*.
      - Loop through index t ranges from n to N - 1, inclusive on both ends.
      - Get the n-gram preceding the word at position t and the word at position t.
      - Estimate the *probability* of the word given the n-gram using the n-gram counts, n-plus1-gram counts,vocabulary size, and smoothing constant.
      - Update the product of the probabilities. This *product_pi* is a cumulative product of the (1/P) factors that are calculated in the loop that is *product_pi *= (1/probability)*.
      - Take the Nth root of the product that is caluclate the *perplexity = (product_pi)**(1/N).*
      - This passed the unit-test case as well.

  - PART 4: Build an Auto-complete System
    - Exercise 11 - suggest_a_word
      - Implemented a function suggest_a_word() that takesprevious_tokens, n_gram_counts, n_plus1_gram_counts, vocabulary, end_token, unknown_token, k=1.0, start_with=None as inputs.
      - Returns a tuple of string of the most likely next word and the corresponding probability.
      - Find the length of previous words and store under n.
      - From the words that the user already typed and get the most recent 'n' words as the **previous n-gram**.
      - Estimate the probabilities that each word in the vocabulary is the next word, given the previous n-gram, the dictionary of n-gram counts,the dictionary of n plus 1 gram counts, and the smoothing constant.
      - Initialized suggested word to None as this will be set to the word with highest probability.
      - Initialized the highest word probability to 0, this will be set to the highest probability of all words to be suggested.
      - Looped through each word and its probability in the probabilities dictionary and check If the optional start_with string is set and check if the beginning of word does not match with the letters in 'start_with' then if they don't match, skip this word (move onto the next word).
      - Check if this word's probability is greater than the current maximum probability then save this word as the best suggestion (so far) and save the new maximum probability.
      - This passed the unit-test case as well.




     
<br><br>

- Partly implemented:
  - w3_unittest.py was not implemented as part of assignment to pass all unit-tests for the graded functions(). It was already created by the Professor.

<br><br>

- Bugs
  - No bugs

<br><br>


## Reflections

- Assignment is very good. Gives a thorough understanding of the basis of Preprocess the Data, n-grams, k-smoothing, Perplexity and as a whole of Autocomplete algorithm.


## Output

### output:

<pre>
<br/><br/>
Note: There was some error while writing s,e,unk inside "<>" in the README.

Out[2] - 

Data type: <class 'str'>
Number of letters: 3335477
First 300 letters of the data
-------
"How are you? Btw thanks for the RT. You gonna be in DC anytime soon? Love to see you. Been way, way too long.\nWhen you meet someone special... you'll know. Your heart will beat more rapidly and you'll smile for no reason.\nthey've decided its more fun if I don't.\nSo Tired D; Played Lazer Tag & Ran A "
-------
Last 300 letters of the data
-------
"ust had one a few weeks back....hopefully we will be back soon! wish you the best yo\nColombia is with an 'o'...“: We now ship to 4 countries in South America (fist pump). Please welcome Columbia to the Stunner Family”\n#GutsiestMovesYouCanMake Giving a cat a bath.\nCoffee after 5 was a TERRIBLE idea.\n"
-------

Out[4] -
I have a pen.
I have an apple. 
Ah
Apple pen.

['I have a pen.', 'I have an apple.', 'Ah', 'Apple pen.']

Expected answer:

['I have a pen.', 'I have an apple.', 'Ah', 'Apple pen.']

Out[5] - All tests passed

Out[7] - 

[['sky', 'is', 'blue', '.'],
 ['leaves', 'are', 'green', '.'],
 ['roses', 'are', 'red', '.']]

Expected output
[['sky', 'is', 'blue', '.'],
 ['leaves', 'are', 'green', '.'],
 ['roses', 'are', 'red', '.']]

Out[8] - All tests passed

Out[10] -

[['sky', 'is', 'blue', '.'],
 ['leaves', 'are', 'green'],
 ['roses', 'are', 'red', '.']]

Expected outcome
[['sky', 'is', 'blue', '.'],
 ['leaves', 'are', 'green'],
 ['roses', 'are', 'red', '.']]

Out[11] - All tests passed

Out[13] - 

47961 data are split into 38368 train and 9593 test set
First training sample:
['i', 'personally', 'would', 'like', 'as', 'our', 'official', 'glove', 'of', 'the', 'team', 'local', 'company', 'and', 'quality', 'production']
First test sample
['that', 'picture', 'i', 'just', 'seen', 'whoa', 'dere', '!', '!', '>', '>', '>', '>', '>', '>', '>']

Expected output
47961 data are split into 38368 train and 9593 test set
First training sample:
['i', 'personally', 'would', 'like', 'as', 'our', 'official', 'glove', 'of', 'the', 'team', 'local', 'company', 'and', 'quality', 'production']
First test sample
['that', 'picture', 'i', 'just', 'seen', 'whoa', 'dere', '!', '!', '>', '>', '>', '>', '>', '>', '>']

Out[15] -

{'sky': 1,
 'is': 1,
 'blue': 1,
 '.': 3,
 'leaves': 1,
 'are': 2,
 'green': 1,
 'roses': 1,
 'red': 1}

Expected output
Note that the order may differ.

{'sky': 1,
 'is': 1,
 'blue': 1,
 '.': 3,
 'leaves': 1,
 'are': 2,
 'green': 1,
 'roses': 1,
 'red': 1}

Out[16] - All tests passed

Out[18] - 

Closed vocabulary:
['.', 'are']

Expected output
Closed vocabulary:
['.', 'are']

Out[19] - All tests passed

Out[21] - 

Original sentence:
[['dogs', 'run'], ['cats', 'sleep']]
tokenized_sentences with less frequent words converted to '<unk>':
[['dogs', '<unk>'], ['<unk>', 'sleep']]

Expected answer
Original sentence:
[['dogs', 'run'], ['cats', 'sleep']]
tokenized_sentences with less frequent words converted to '<unk>':
[['dogs', '<unk>'], ['<unk>', 'sleep']]

Out[22] - All tests passed

Out[24] - 

tmp_train_repl
[['sky', 'is', 'blue', '.'], ['leaves', 'are', 'green']]

tmp_test_repl
[['<unk>', 'are', '<unk>', '.']]

tmp_vocab
['sky', 'is', 'blue', '.', 'leaves', 'are', 'green']

Expected outcome
tmp_train_repl
[['sky', 'is', 'blue', '.'], ['leaves', 'are', 'green']]

tmp_test_repl
[['<unk>', 'are', '<unk>', '.']]

tmp_vocab
['sky', 'is', 'blue', '.', 'leaves', 'are', 'green']

Out[25] - All tests passed

Out[27] - 

First preprocessed training sample:
['i', 'personally', 'would', 'like', 'as', 'our', 'official', 'glove', 'of', 'the', 'team', 'local', 'company', 'and', 'quality', 'production']

First preprocessed test sample:
['that', 'picture', 'i', 'just', 'seen', 'whoa', 'dere', '!', '!', '>', '>', '>', '>', '>', '>', '>']

First 10 vocabulary:
['i', 'personally', 'would', 'like', 'as', 'our', 'official', 'glove', 'of', 'the']

Size of vocabulary: 14824

Expected output:
First preprocessed training sample:
['i', 'personally', 'would', 'like', 'as', 'our', 'official', 'glove', 'of', 'the', 'team', 'local', 'company', 'and', 'quality', 'production']

First preprocessed test sample:
['that', 'picture', 'i', 'just', 'seen', 'whoa', 'dere', '!', '!', '>', '>', '>', '>', '>', '>', '>']

First 10 vocabulary:
['i', 'personally', 'would', 'like', 'as', 'our', 'official', 'glove', 'of', 'the']

Size of vocabulary: 14821

Out[29] -  

Uni-gram:
{('s',): 2, ('i',): 1, ('like',): 2, ('a',): 2, ('cat',): 2, ('e',): 2, ('this',): 1, ('dog',): 1, ('is',): 1}
Bi-gram:
{('s', 's'): 2, ('s', 'i'): 1, ('i', 'like'): 1, ('like', 'a'): 2, ('a', 'cat'): 2, ('cat', 'e'): 2, ('s', 'this'): 1, ('this', 'dog'): 1, ('dog', 'is'): 1, ('is', 'like'): 1}

Expected outcome:

Uni-gram:
{('s',): 2, ('i',): 1, ('like',): 2, ('a',): 2, ('cat',): 2, ('e',): 2, ('this',): 1, ('dog',): 1, ('is',): 1}
Bi-gram:
{('s', 's'): 2, ('s', 'i'): 1, ('i', 'like'): 1, ('like', 'a'): 2, ('a', 'cat'): 2, ('cat', 'e'): 2, ('s', 'this'): 1, ('this', 'dog'): 1, ('dog', 'is'): 1, ('is', 'like'): 1}

Out[30] - All tests passed

Out[32] - 

The estimated probability of word 'cat' given the previous n-gram 'a' is: 0.3333

Expected output
The estimated probability of word 'cat' given the previous n-gram 'a' is: 0.3333

Out[33] - All tests passed

Out[35] - 

{'dog': 0.09090909090909091,
 'is': 0.09090909090909091,
 'this': 0.09090909090909091,
 'i': 0.09090909090909091,
 'cat': 0.2727272727272727,
 'a': 0.09090909090909091,
 'like': 0.09090909090909091,
 'e': 0.09090909090909091,
 'unk': 0.09090909090909091}

Expected output
{'cat': 0.2727272727272727,
 'i': 0.09090909090909091,
 'this': 0.09090909090909091,
 'a': 0.09090909090909091,
 'is': 0.09090909090909091,
 'like': 0.09090909090909091,
 'dog': 0.09090909090909091,
 'e': 0.09090909090909091,
 'unk': 0.09090909090909091}

Out[36] - 

{'dog': 0.09090909090909091,
 'is': 0.09090909090909091,
 'this': 0.18181818181818182,
 'i': 0.18181818181818182,
 'cat': 0.09090909090909091,
 'a': 0.09090909090909091,
 'like': 0.09090909090909091,
 'e>': 0.09090909090909091,
 'unk': 0.09090909090909091}

Expected output
{'cat': 0.09090909090909091,
 'i': 0.18181818181818182,
 'this': 0.18181818181818182,
 'a': 0.09090909090909091,
 'is': 0.09090909090909091,
 'like': 0.09090909090909091,
 'dog': 0.09090909090909091,
 'e>': 0.09090909090909091,
 'unk>': 0.09090909090909091}

Out[38] - 
bigram counts
       dog	is	this	i	cat	a	like	e>	unk>
(like,)	0.0	0.0	0.0	0.0	0.0	2.0	0.0	0.0	0.0
(i,)	  0.0	0.0	0.0	0.0	0.0	0.0	1.0	0.0	0.0
(cat,)	0.0	0.0	0.0	0.0	0.0	0.0	0.0	2.0	0.0
(dog,)	0.0	1.0	0.0	0.0	0.0	0.0	0.0	0.0	0.0
(s,)	0.0	0.0	1.0	1.0	0.0	0.0	0.0	0.0	0.0
(this,)	1.0	0.0	0.0	0.0	0.0	0.0	0.0	0.0	0.0
(a,)	  0.0	0.0	0.0	0.0	2.0	0.0	0.0	0.0	0.0
(is,)	  0.0	0.0	0.0	0.0	0.0	0.0	1.0	0.0	0.0

Expected output
bigram counts
          cat    i   this   a  is   like  dog  e>   unk>
(s>,)    0.0   1.0  1.0  0.0  0.0  0.0   0.0  0.0    0.0
(a,)      2.0   0.0  0.0  0.0  0.0  0.0   0.0  0.0    0.0
(this,)   0.0   0.0  0.0  0.0  0.0  0.0   1.0  0.0    0.0
(like,)   0.0   0.0  0.0  2.0  0.0  0.0   0.0  0.0    0.0
(dog,)    0.0   0.0  0.0  0.0  1.0  0.0   0.0  0.0    0.0
(cat,)    0.0   0.0  0.0  0.0  0.0  0.0   0.0  2.0    0.0
(is,)     0.0   0.0  0.0  0.0  0.0  1.0   0.0  0.0    0.0
(i,)      0.0   0.0  0.0  0.0  0.0  1.0   0.0  0.0    0.0

Out[39] - 

trigram counts
          dog	 is	 this	 i	cat	a	 like	e>	unk>
(i, like)	 0.0	0.0	0.0	0.0	0.0	1.0	0.0	0.0	0.0
(dog, is)	 0.0	0.0	0.0	0.0	0.0	0.0	1.0	0.0	0.0
(s>, i)	 0.0	0.0	0.0	0.0	0.0	0.0	1.0	0.0	0.0
(s>, s>)	0.0	0.0	1.0	1.0	0.0	0.0	0.0	0.0	0.0
(s>, this)	1.0	0.0	0.0	0.0	0.0	0.0	0.0	0.0	0.0
(is, like)	0.0	0.0	0.0	0.0	0.0	1.0	0.0	0.0	0.0
(like, a)	  0.0	0.0	0.0	0.0	2.0	0.0	0.0	0.0	0.0
(this, dog)	0.0	1.0	0.0	0.0	0.0	0.0	0.0	0.0	0.0
(a, cat)	  0.0	0.0	0.0	0.0	0.0	0.0	0.0	2.0	0.0

Expected output
trigram counts
              cat    i   this   a  is   like  dog  e>   unk>
(dog, is)     0.0   0.0  0.0  0.0  0.0  1.0   0.0  0.0    0.0
(this, dog)   0.0   0.0  0.0  0.0  1.0  0.0   0.0  0.0    0.0
(a, cat)      0.0   0.0  0.0  0.0  0.0  0.0   0.0  2.0    0.0
(like, a)     2.0   0.0  0.0  0.0  0.0  0.0   0.0  0.0    0.0
(is, like)    0.0   0.0  0.0  1.0  0.0  0.0   0.0  0.0    0.0
(s>, i)      0.0   0.0  0.0  0.0  0.0  1.0   0.0  0.0    0.0
(i, like)     0.0   0.0  0.0  1.0  0.0  0.0   0.0  0.0    0.0
(s>, s>)    0.0   1.0  1.0  0.0  0.0  0.0   0.0  0.0    0.0
(s>, this)   0.0   0.0  0.0  0.0  0.0  0.0   1.0  0.0    0.0

Out[41] - 
bigram probabilities
        dog	       is	       this	      i	       cat	       a	      like	    <e>	     <unk>
(like,)	0.090909	0.090909	0.090909	0.090909	0.090909	0.272727	0.090909	0.090909	0.090909
(i,)	  0.100000	0.100000	0.100000	0.100000	0.100000	0.100000	0.200000	0.100000	0.100000
(cat,)	0.090909	0.090909	0.090909	0.090909	0.090909	0.090909	0.090909	0.272727	0.090909
(dog,)	0.100000	0.200000	0.100000	0.100000	0.100000	0.100000	0.100000	0.100000	0.100000
(<s>,)	0.090909	0.090909	0.181818	0.181818	0.090909	0.090909	0.090909	0.090909	0.090909
(this,)	0.200000	0.100000	0.100000	0.100000	0.100000	0.100000	0.100000	0.100000	0.100000
(a,)	  0.090909	0.090909	0.090909	0.090909	0.272727	0.090909	0.090909	0.090909	0.090909
(is,)	  0.100000	0.100000	0.100000	0.100000	0.100000	0.100000	0.200000	0.100000	0.100000

Out[42] - 
trigram probabilities
           dog	         is	      this	     i	       cat	      a	      like	     <e>	   <unk>
(i, like)	 0.100000	0.100000	0.100000	0.100000	0.100000	0.200000	0.100000	0.100000	0.100000
(dog, is)	 0.100000	0.100000	0.100000	0.100000	0.100000	0.100000	0.200000	0.100000	0.100000
(<s>, i)	 0.100000	0.100000	0.100000	0.100000	0.100000	0.100000	0.200000	0.100000	0.100000
(<s>, <s>) 0.090909	0.090909	0.181818	0.181818	0.090909	0.090909	0.090909	0.090909	0.090909
(<s>, this)0.200000	0.100000	0.100000	0.100000	0.100000	0.100000	0.100000	0.100000	0.100000
(is, like) 0.100000	0.100000	0.100000	0.100000	0.100000	0.200000	0.100000	0.100000	0.100000
(like, a)	 0.090909	0.090909	0.090909	0.090909	0.272727	0.090909	0.090909	0.090909	0.090909
(this, dog)0.100000	0.200000	0.100000	0.100000	0.100000	0.100000	0.100000	0.100000	0.100000
(a, cat)	 0.090909	0.090909	0.090909	0.090909	0.090909	0.090909	0.090909	0.272727	0.090909

Out[44] - 

Perplexity for first train sample: 2.8040
Perplexity for test sample: 3.9654

Out[45] - All tests passed
Expected Output
Perplexity for first train sample: 2.8040
Perplexity for test sample: 3.9654

Out[47] - 

The previous words are 'i like',
	and the suggested word is `a` with a probability of 0.2727

The previous words are 'i like', the suggestion must start with `c`
	and the suggested word is `cat` with a probability of 0.0909

Expected output
The previous words are 'i like',
    and the suggested word is `a` with a probability of 0.2727

The previous words are 'i like', the suggestion must start with `c`
    and the suggested word is `cat` with a probability of 0.0909

Out[50] - 

The previous words are 'i like', the suggestions are:
[('a', 0.2727272727272727),
 ('a', 0.2),
 ('dog', 0.1111111111111111),
 ('dog', 0.1111111111111111)]

Out[51] - 

Computing n-gram counts with n = 1 ...
Computing n-gram counts with n = 2 ...
Computing n-gram counts with n = 3 ...
Computing n-gram counts with n = 4 ...
Computing n-gram counts with n = 5 ...

Out[52] - 

The previous words are ['i', 'am', 'to'], the suggestions are:
[('be', 0.027662668120683388),
 ('have', 0.00013484358144552318),
 ('have', 0.0001348799568384138),
 ('i', 6.744907594765952e-05)]

Out[53] - 

The previous words are ['i', 'want', 'to', 'go'], the suggestions are:
[('to', 0.01404932875429285),
 ('to', 0.004697009790949987),
 ('to', 0.000942253331538565),
 ('to', 0.0004043671653861706)]

Out[54] - 
The previous words are ['hey', 'how', 'are'], the suggestions are:
[('you', 0.02342280731749017),
 ('you', 0.0035587188612099642),
 ('you', 0.00013488905375328792),
 ('i', 6.744907594765952e-05)] 

Out[55] - 

The previous words are ['hey', 'how', 'are', 'you'], the suggestions are:
[("'re", 0.023971072197619143),
 ('?', 0.002887897085849304),
 ('?', 0.0016131200430165346),
 ('<e>', 0.00013488905375328792)]

Out[56]-

The previous words are ['hey', 'how', 'are', 'you'], the suggestions are:
[('do', 0.009019623776053306),
 ('doing', 0.001640850616959832),
 ('doing', 0.00047049334587982255),
 ('dvd', 6.744452687664396e-05)]  
<br/><br/>
</pre>
