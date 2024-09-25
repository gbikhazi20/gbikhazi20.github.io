# What are Word Embeddings and What Makes Them so Cool?

Everyday, we hear more and more about computer programs that are capable of processing language. Tools like Google Search, Siri, and ChatGPT are all built with natural language processing (NLP) technology. These three tools in particular seem to hold some sort of semantic understanding of the words we provide them with — we can word commands or prompts differently and achieve the same results. For example, the phrases "How is the weather today?" and "What is the temperature outside?" are worded differently, but we would expect similar results from a Google search.

So how is it that computers are seemingly able to grasp the varied things we try to say to them? This question becomes especially perplexing when considering the fact that, beneath all of its abstractions, a computer only has a concept of numbers (more specifically 1s and 0s, and even more specifically, electrical signals that are either on or off). Perhaps unsurprisingly, the answer involves numbers.

Suppose we wanted to represent each word in the English language by a number that gave some indication to its degree of expressing the concept of heat. If we can do so successfully, then a computer will be able to capture some sort of meaning in each word (which it wouldn't be able to otherwise, when words are just strings).

<br/>

<p align="left">
<img src="https://raw.githubusercontent.com/gbikhazi20/gbikhazi20.github.io/main/_assets/word-chart-1.jpg" width="500" height="200">
</p>

<br/>

We can come up with some numbers that feel right for words like freezing, cool, tepid, warm, and scalding. But what do we do about words that aren't really related to heat in their meaning? What do we do about words we use to describe size?

Well, we could add another axis.

<br/>

<p align="left">
<img src="https://raw.githubusercontent.com/gbikhazi20/gbikhazi20.github.io/main/_assets/word-chart-2.jpg" width="500" height="300">
</p>

<br/>

At first glance, this seems to work. Our temperature words span the temperature axis and have a value of 0 on the size axis, and vice versa for our size words.

However, we soon run into the same issue we dealt with the first time. We can easily come up with plenty of words that don't fit anywhere in this two dimensional space. We can also keep adding axes as we see fit. But when would we stop adding axes? We could easily end up with dozens of thousands of axes, each one used to describe some specific attribute through which we describe all the things that make up our world. Also, wouldn't it be tedious to do this all by hand? And how would we know our numbers are accurate and representative of others' opinions of the same words?

#### Enter: word embedding algorithms.

As you might have guessed, there are methods in computer science to automate this process and make it more deliberate and precise. There are a number of popular techniques used in industry and academia, namely Word2Vec, GloVe, and FastText.

I won't get too much into how it is exactly these techniques work, other than that some require training neural networks and others rely on statistical analyses of corpora using word co-occurrences (looking at which words appear near each other in texts). These mechanisms which produce word embeddings are fascinating and I would highly recommend looking into how they work. Now, I would like to focus the rest of this article on the interesting characteristics of word embeddings.

As was hinted at in the first example, the idea for word embeddings is to encapsulate some sort of semantic meaning for words. In our two-dimensional example, the word "scalding" would be represented by the vector (12, 0) since its value on the x-axis (which described heat) was 12 and its value on the y-axis (which described size) was 0. We have knowledge of what each dimension or axis is tracking. However, when we have a 100-dimensional vector produced by GloVe, we can't exactly crack it open and determine what each axis is describing. The dimensions of the vector space are essentially abstract numerical representations that capture various aspects of the word's usage and context.

This description of word embeddings may make them seem unintuitive, but this is not necessarily the case. Certain embeddings track semantic meaning really well. For example, with Word2Vec embeddings, taking the vector for the word "king", subtracting from the vector for the word "man" and adding to it the vector for the word "woman" results in a vector very close to that for the word "queen". It seems our embeddings have managed to learn the male-female relationship.

In some sense, we can now describe words with numbers, meaning computers have access to a representation for words that they are able to process. Because of this, tasks like clustering, classification, translation, retrieval, and more can be made much more accurate.

Word embeddings are massively beneficial to computer programs and have allowed for breakthroughs in the development of recent technologies. It is important to consider the possible shortcomings of such a technology, though.

Word embedding algorithms typically require massive amounts of text to be produced, and massive amount of text have the potential to contain biases. The same embeddings that are able to associate the relationship between men and women and kings and queens also have the potential to contain sexist relationships. For instance, taking the word vector for "computer programmer", subtracting from it the vector for "man" and adding to it the vector for "woman" yields a vector close to that for "homemaker". Obviously, this is problematic.

Suppose we had an AI that analyzed resumes by building document-level embeddings based off of word embeddings for the words making up the resume. If the AI was programmed to move along candidates in a certain neighborhood of whatever dimension we are working in, it might penalize women if it builds a representation for their profiles further away from "programmer" and closer to "homemaker".

This is a dangerous aspect of word embeddings, and luckily there are techniques for reducing bias in them. Nonetheless, it is very important to be careful with word embeddings and more generally any AI that builds representations of the world based on biased data.

### Sources

https://papers.nips.cc/paper_files/paper/2016/file/a486cd07e4ac3d270571622f4f316ec5-Paper.pdf

