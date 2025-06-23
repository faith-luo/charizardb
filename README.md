# charizardb
Japanese-Chinese cross-linguistic (k|h)an(j|z)i mapping

## Overview

I am a native bilingual English/Chinese speaker. When I started learning Japanese, I found that most language learning resources were targeted either at Chinese speakers or English speakers. There were not a lot of CN/EN/JP resources out there. So I set out to create some on my own. There are enough people in similar situations (Chinese people who have studied English and now want to learn Japanese, Asian-Americans who knows some Chinese studying Japanese) that I figured it might be a helpful resource if I shared it with the public.

Really, the key part of this is that I'd like to be able to see the Japanese variant of a Chinese character (hanzi), and the Chinese variant of a Japanese character (kanji) whenever I look up an English definition.

It's pretty easy to find Japanese-English dictionaries, and Chinese-English dictionaries. You can probably dig out Chinese-Japanese or Japanese-Chinese dictionaries with enough elbow grease and search engine-foo, but they're not easily available to English speakers (which defies needing them in the first place). But the problem with these bilingual-only dictionaries is that they don't contain words that don't exist in the target language. For instance, a Chinese-English dictionary would never contain the word 丼 because it only exists in Japanese. And a Chinese-Japanese dictionary would probably translate it to 碗 instead of leaving it empty, which is what I want.

For whatever reason I've had a hard time finding this. The best I've found is opencc's [dataset](https://github.com/BYVoid/OpenCC/blob/master/data/dictionary/JPVariants.txt). But it has some issues:
* It's implemented in C++, which is fast but isn't a great scripting language
* It also only has 369 entries
* It only contains variants, and doesn't contain Japanese-only or Chinese-only characters

(It turns out that 369 is probably not that far from the real number of variants, since 90% of Japanese characters just match traditional Chinese variants. With my techniques I found 516 on the 3000 most common characters.)

Once we have this data we can also make a list of all sino-japanese words (i.e. words that equivalent across characters, like 可憐/可怜=かれん=ke lian). 

In any case, I'm sharing my data here. Hopefully someone else finds it useful.

## Contents
* `characters_common.txt` - A union of Jinmeiyo kanji, Joyo kanji, JLPT kanjk, HSK hanzi, and the 3000 most common characters from both Chinese and Japanese. Contains Japanese/simplified CN/trad CN variants, pinyin readings, kun/on readings, and basic english meaning.
* `characters_all.txt` - Same as above but contains around 10000 charact    ers from each language.
* `variants_common.txt` - Only the characters in the common list that differ between Japanese and chinese traditional. Useful for benchmarking against other dictionaries.
* `variants_all.txt` - A Variants but for the long list.
* `sino_jp.txt` - Using the above data, a list of around 20000 sino-jp words and their pinyin/hiragana readings.

## Techniques
I'll do a proper writeup at some point, but most of the data is sourced from Unihan. The tl;dr is this:
* Do a DFS on the map of all unihan connections to find all clusters
* Since clusters tend to be too noisy on their own (they contain obselete and obscure variants), filter and reduce based on various heuristics, such as if the character appears in the 10000 most common words in a dictionary
* Group characters that have similar pinyin and semantic meanings as "merge candidates"
* Merge characters that pass a certain heuristic

The source code is available at [faith-luo/make-anki-deck](https://github.com/faith-luo/anki-make-kanji-deck) but it's very disorganized.