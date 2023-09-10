# Hangman Game Project

So for this project I focused on interactions between letters within words.
Originally I looked to find ways to predict words based on each individual letter at each individual place. With little success, I turned to examining groups of letters together. I remember reading an article about hangman tips
and one key point they made was that you can kind of tell what words could potentially exist. For example look at the two following combination of letters: Zurvinity and Jrajussnh. While the latter has more frequentlt appearing
letters, the latter looks more like a potential word and with out of sample analysis we would more likely want to make a guess like the first rather than the second one. Through initial testing, I noticed I needed to treat vowels
differently. Previous works on this question I saw used a simple metric for dealing with too many vowels by reducing the weights on vowels if majority of the word is made up of vowels. I added this metric into mine. However, I also
added in a potential issue with smaller words (this will be discussed further down) where I found getting at least 1 vowel into the word helps the main algo to start working so I had it guess vowels until at least 1 vowel in the word
was found.

While the main algo is time consuming, the initial strat that the code gives me is quite useful in getting the general form of the word with great speed and decent results. I let the frequency determining method orignially put out
to begin the search and once enough of the word is found, let the main algo take over (around 2/3 left to find).

For the main strategy, I look to take all subphrases (len 2 to 6) within each word in the training dictionary to find best fitting phrases in the target word. I then split them up based on how prefixes/suffixes/base word by splitting string up into thirds
and the first third is prefix, last third is suffix and middle is the base word. This generally helps with the larger words and has great success above 60-90% for words length 9 or longer in training but much less so for the shorter words. Now to
match these phrases into the current word, I then need to get all the subphrases of the current word that we need to examine.
For each missing values in target word, I look to find all potential subphrases that could work. For example: .pp.e as .p, .pp, .pp., .pp.e, pp., pp.e, p., p.e, .e. However thats not all. I further categorize these phrases into the 3 buckets from
before of (prefix/suffix/base) in that given the word is long enough (7 or greater) where we only put phrase into prefix lst if it starts with the first place of the target word and likewise suffix and last place of the word. Lastly,
I combine these two in an iterative method and with weights that emphasises the number of frequency the subphrase in the training dictionary, the length of the word, and the number of letters that it matches. Before this, per iteration, I get rid
of the subphrases dictionary of words that no longer work given guessed letters and new phrases to form. From there comining these two, I can get all the subphrases that work with weights based on previous crieria, sort them and get the most frequent letter
not guessed and return that letter. If this returns empty, I fall back to the precreated code on taking most frequent letter in the full dict thats left.

Now back the issue that is still an issue. The shorter words seem to still to terrible here. With no guesses, we always start with the same thing. For len(5) it's ..... . The problem here is that with fewer letters and less pattern following, we got lucky in some sense.
A big issue is that these words sometimes don't have patterns to follow and thus getting the right out of sample is harder. I tried towards the end to try and implement a method that extracts words from larger words by removing prefix and suffix and add these
to the dictionary, but with lots of noice could not get as far as I'd like to there. Ideally for example, disreguardless through this function gets reguard and adds to the dictionary.
