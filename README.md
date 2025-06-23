# charizardb
Japanese-Chinese cross-linguistic (k|h)an(j|z)i mapping

## Overview

I am a native bilingual English/Chinese speaker. When I started learning Japanese, I found that most language learning resources were targeted either at Chinese speakers or English speakers. There were not a lot of CN/EN/JP resources out there. So I set out to create some on my own. There are enough people in similar situations (Chinese people who have studied English and now want to learn Japanese, Asian-Americans who knows some Chinese studying Japanese) that I figured it might be a helpful resource if I shared it with the public.

Really, the key part of this is that I'd like to be able to see the Japanese variant of a Chinese character (hanzi), and the Chinese variant of a Japanese character (kanji) whenever I look up an English definition.

It's pretty easy to find Japanese-English dictionaries, and Chinese-English dictionaries. You can probably dig out Chinese-Japanese or Japanese-Chinese dictionaries with enough elbow grease and search engine-foo, but they're not easily available to English speakers (which defies needing them in the first place). Also, they tend to have a lot of excess data. I don't need a list of every Chinese word ever, I just want the 3000-4000 most common characters in both languages and a mapping between equivalences, in an easily readable format.

For whatever reason I've had a hard time finding this. The best I've found is opencc's [dataset](https://github.com/BYVoid/OpenCC/blob/master/data/dictionary/JPVariants.txt). But it's a bit clunky to use because it's implemented in C++, and it also only has 369 entries.

It turns out this is probably not that far from the real number, 90% of Japanese characters just match traditional Chinese variants. But with my techniques I was able to find 516. (They are not verified 1-1 though, so maybe it is a less consistent list than OpenCC's.)

Once we have this data we can also make a list of all sino-japanese words (i.e. words that equivalent across characters, like 可憐/可怜=かれん=ke lian). 

In any case, I'm sharing my data here. Hopefully someone else finds it useful.

## Techniques
I'll do a proper writeup at some point, but most of the data is sourced from Unihan. The tl;dr is this:
* Do a DFS on the map of all unihan connections to find all clusters
* Since clusters tend to be too noisy on their own (they contain obselete and obscure variants), filter and reduce based on various heuristics, such as if the character appears in the 10000 most common words in a dictionary
* Group characters that have similar pinyin and semantic meanings as "merge candidates"
* Merge characters that pass a certain heuristic
