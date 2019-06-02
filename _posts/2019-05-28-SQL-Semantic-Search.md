---
published: False
---
One of the many applications of sentence embeddings is semantic search. 

Given a document $d$, find the $k$ most "related" documents to $d$. 

We can implement this as kNN search, there's also third-party libraries that do exactly this (see SPTAG from Microsoft). 

But I think these approaches are a bit overkill for what I imagine is the most common usecase:

We have somewhere around 1M-10M documents, unlabeled, just free form responses, and we want to look for trends in these documents, each document is about 10 sentences. 

The first thing we need to do is to generate a vector representation of our documents. 
We could train sentence representations, but to make things a bit simpler, we'll use a pretrained embedding that you can find [here]:

With this in mind, we know just need to find the $k$ such that $w_k^Td$ is maximized. 

The problem is that if we're searching over 10M documents, that won't all fit in memory, so we can't just keep a big matrix in memory and do a big matrix multiplication. Assuming our embedding dimension is 300, that means we would have a (100M, 300) matrix of doubles. Each double would be 8 bytes, for a total of 24 Gigabytes, a bit too large to fit comfortably into memory. 

You can store vector embeddings of comments in a table inside a database. In fact, we can do this quickly all inside the database easily by using a bit of SQL. 

I've written a little bit of wrapper code that brings up a PSQL database, and then encodes all the comments and stores all this information inside the database. 

In fact, we can do this quickly all inside the database easily by using a bit of SQL. 

Still it looks like such an approach works. 
