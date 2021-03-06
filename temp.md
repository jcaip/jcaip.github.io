---
layout: page
permalink: /temp/
---

Stuff I wanna remember

---
# DistBelief Part 2

1. First, we tried to use openmpi to run our code - good starting point. 
2. still need to profile code

I should be able to use starcluster to spin up an OpenMPI cluster. 

I just need to change the model to use model averaging instead of this param-request approach. 
also need to figure out their bipartite graph shit

- shouldn't use a gradient norm update.

something about ill conditioning

---
# Wasserstein Distance

https://www.youtube.com/watch?v=aEOuu75z694

## Notes about optimal transport

kl divergence measures overlap, but if they don't overlap at all we gain no more information. 

EMD/ Wassertien distance 

can we reformat this as a continuous problem? 

divergence grows linearly with how far the distribution is from each other. 

### Monge-Kantorovich Problem 

finding distance between mu and nu

X could be a mainfold/euclidean space 

Wasserstein barycenters provides good platonic form of MNIST data.

---
# Building The Reals $\mathbb{R}$

Unfortunately we can't use the same approach to construct $\mathbb{R}$. While the cardinality of the $\mathbb{N}, \mathbb{Z}, \mathbb{Q}$ are all equal, $\mathbb{R}$ is far more dense.

Instead we'll consider a way to partition the $\mathbb{Q}$ number line called a Dedekinde cut. 

More precisely, a subset $r$ of $\mathbb{Q}$ is a cut if it satisfys these conditions: 

1. $r \neq \emptyset \land r \neq \mathbb{Q}$
3. $\forall x \in r .\forall y \in \mathbb{Q}: x \leq y \implies x \in r$
4. $\forall x \in \mathbb{Q} \setminus A. \exists y \in \mathbb{Q}: x < y \land y \not \in A$

$\mathbb{R}$ is then defined as the set of all cuts of $\mathbb{Q}$.

$$\mathbb{R} = \{ r \subset \mathbb{Q}: r \text{ is a cut }\}$$

With this in mind let's look at the cut that represents the rational number $1$ in the reals. Again please note the axes should extend to $\pm \infty$.

![r1](/images/R/R1.png){: .center}

And also the irrational number $\sqrt 2$.

![r2](/images/R/Rroot2.png){: .center}

### Arithmetic on the reals
We can define a well-ordering $\leq$ on the reals such that $\forall x,y \in \mathbb{R}: x \leq y  \iff x \subset y$. 

Visually, we can see that the cut representing $1$ is indeed a subset of $\sqrt 2$, as $1 \leq \sqrt2$

We can define addition simply as

$$ A + B  = \{a + b: a \in A \land b \in B \}$$

So adding together $ 1 + \sqrt 2$ looks something like this:

![raadd](/images/R/Raddition.png){: .center}

### Parsing Notes CS269 05/06/19

Language structure - Words take different arguments

Intransitive/transitive/ditransitive verbs that take in additional object

- Language is recursive, so model with a DFA/ graph, with complex constituents.

- syntatically, you can replace one noun phrase with another.

complex constituents - Group of words that behave as a single unit.
- phrases usually have a **head** and some **dependents**
    - dependents can be arguments (mandatory) or adjuncts (optional)

structure tree is dependent on the words - "eat sushi with tuna" vs. "eat sushi with chopsticks"

Dependency parsing - mostly projective in English, but can be free form in other languages.

- Graph Algorithms
    - consider all word pairs, assign a scores
    - score of a tree = sum of score of edges
- Transition-based Approachs
    - similar to a parser for code (shift-reduce parser)
    - parser has a stack (starting with root) and buffer (originally the sentence)
    - use a set of actions to parse sentences
    - allow a subset of actions - for example: Arc-Eager Dependency Parser

 approaches use DNN to capture features
 train a classifier to make an action at any time - kind of like RL?

 dependencies can be represented as a tree by definition.

 A dependency graph is projective if there isn't a path from target to dependent.

 Covington's D-rules
- this is linear in time O(n), because it's just one pass through
measured loss is basically Hamming loss.

Phrase-structure / dependency parse trees

Chomsky Normal Form - only alllows either two non-terminals or one-terminal.

Parsing CNF grammars - CKY algorithm, parses as a matrix, every cell stores constituents found between i and j.

O(n^3 * G)

Finding the most probable parse tree - probabilty of a tree is the probability of its children:

$$P(T) = \prod_{\tau \in T} P(\tau) $$

Likelihood
Inference (Decoding)
Learning (Estimation)

Grammar induction - learn the rules that make the sentence most probable.
Generate more specific grammar rules using head word information. 

Dynamic oracle - use left most split, O(n^2) greedy algorithm.

### Machine Translation

Originally rule based, but now probabilistic. 

$$\argmax_y P( y \mid x ) = argmax_y P(x \mid y) P(y)$$

P(y) can be thought of as language model, or how to model good english.

Parallel data - i.e. Rosetta Stone

Document level alignment as well - Wikipedia. Also need to consider alignment, not just words. 

Neural Machine Translation - use Encoder-Decoder RNN framework. Decoder is a language model, added in self-attention.

Decoding: Cannot enumerate all sequences.  O(V^k)
- can use greedy decoding
    - but this makes recovering from a mistake almost impossible. 
- or use a sampling strategy at each step
- use beam search
    - define beam size, make optimal choice for all step in the beam. 
    - O(k * B * V)

Commpositional vector grammars, can capture syntactic/semantic ambiguity

You can score trees with a standard RNN.

## Information Extraction and Coreference Resolution.

Use NLP to extract a knowledge base you can query against. 

One particular relation - coreference resolution. 

Task 1: identify all mentions
    - pronouns (via POS tag)
    - named entities (NER)
    - noun phrases (use parse tree to get phrases)


mentions - use seq2seq
or train a binary classifier based on the noun phrase. 

- similar to antecedent/anaphor resolution

This problem is also very apparent in the case of Winograd Schema, where there's no good solution. 

Coreference evaluation - MUC, CEAF, LEA, B-CUBBED, BLANC

B-CUBED - For each mention, compute a precision and recall, then take an average

Learn a pairwise similarity measure. (build a binary classfier)
    - this is similar to quickthoughts
how can i use this classifier to build a cluster

You can pick many different featuresss

Build cluster via greedy best-left-link clustering. 
Decoupling may lose information, 

All-links clustering
- Do some combinatorial optimization problem, but this makes inference too hard.  (NP Hard)

Integer learning program, consider it as a structured perceptron. 

Make prediction of forest , then take loss wrt the true latent forest (by comparing score)

## Information Extraction continued

Semantic Role Labeling - Who did what to whom?

PropBank, predicates and arguments. 

POS tagging, and then train a classifier. 

Identify frames via BOW encoder. 

Define a semantic rule based on a theme or something.

Granularity is an issue

1. Fewer, more generalized semantic roles. 
2. Define roles specific to a group of predicates.

#### Role labeling

best matching between spans and roles, conduct constrained inference. 

semantic parsing - 

Logic - lambda operation over a sentence. 

# Fairness and Bias in NLP
