# 101explorer

## General description
101explorer is a web service rooted in the idea of Linked Data and based the computations done by [101worker](https://github.com/101companies/101docs/tree/master/worker). The key idea is to assign every entity in 101companies a unique URI in order to be able to refer to it. The URI should yield all available information about the entity. 

As an example, consider a contribution - dereferencing such an URI should yield information such as the name, but also contained directories or files. Referencing a directory in this contribution should then yield the list of files in this directory. Referencing the file should yield information, such as the language used in this file (e.g. Java) and, if possible, the actual text. 

A novelty of 101worker is the inclusion of [Fact Extraction](https://github.com/101companies/101docs/blob/master/worker/Fact%20Extraction.pdf). Files can be split up even further into fragments, where dereferencing yields only the text of that fragment. This allows referencing of specific classes or methods from [101wiki]().

101explorer data is generally available as HTML, intended for human consumption,  and as machine-readable JSON. 

## Current status

101explorer was available under the URI (http://101companies.org/resources/). The configuration is broken. Also, there are probably multiple problems within the source code as many things in [101worker](https://github.com/101companies/101docs/tree/master/worker) changed since the last modification of 101explorer. 

## Technical information

101explorer is written in Python. Originally, it build directly on [mod_wsgi](http://en.wikipedia.org/wiki/Mod_wsgi). However, we introduced [Django](http://en.wikipedia.org/wiki/Django_%28web_framework%29) as a layer between mod_wsgi and the web service for simplified programming. 

The main work of 101explorer is to transform the given URI and collect the necessary information, which is usually scattered over multiple files due to the way  [101worker](https://github.com/101companies/101docs/tree/master/worker) computes and stores its results. 

## Further reading

101explorer has been the result of a [master thesis](http://softlang.uni-koblenz.de/LeinbergerThesis.pdf).