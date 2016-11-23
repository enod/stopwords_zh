### stopwords_zh

StopWords for Chinese: collect Chinese stopwords, Just for removing common useless words. 
Added traditional chinese stopwords using mafan the converter.


#### Use

You can use for [jieba](https://github.com/fxsjy/jieba) and other Chinese text segmentation, just compare the word whether in the list or not.

Python code:

	#! /usr/bin/env python
	# encoding: utf-8
	import codecs
	import jieba
	
	if __name__ == "__main__":
		str_in = "小明硕士毕业于中国科学院计算所，后在日本京都大学深造"
    	stopwords = codecs.open('stopwords', 'r', 'utf-8').read().split(',')
    	seg_list = jieba.cut_for_search(str_in)
	    for seg in seg_list:
	        if seg not in stopwords:
	            print seg
		
### Link

+ [Baidu Stopwords](http://www.baiduguide.com/baidu-stopwords/)

### Traditional chinese stopwords

	import mafan, codecs
	from mafan import simplify, tradify
	stopwords = codecs.open('./simp_ch_stopwords.txt', 'r', 'utf-8').read().split(',')
	trad_stopwords = [tradify(word).replace('\n', '').strip() for word in stopwords]

### Using it with jieba on dataframe.

Basically just using df.apply(lambda row: tokenizer(row), axis=1) does all the job. 

def tokenizer(row):
    if len(row.article) > 0:
        tokenized = jieba.lcut(''.join(row.article), cut_all=True)
        for seg in tokenized:
            if seg not in stopwords:
                return tokenized 
    else:
        return '' # np.nan if you would like to.

Example: filtered['token'] = filtered.apply(lambda row: tokenizer(row), axis=1) 
