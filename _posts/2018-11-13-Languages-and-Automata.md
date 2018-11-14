
Some notes for CS181, starting with exam 3.

## Context-Free Grammars

A context-free grammar is represented by a tuple $( V, \Sigma, R, S)$ where
* $V$ is the set of all variables
* $\Sigma$ is the alphabet
* $R$ are all the derivation rules (i.e. $S \rightarrow aSb \mid \epsilon$)
* $S$ is the start variable

At each step in time, you can replace one of the variables with one of it's derivations.
The language of a grammar is the set of all strings that can be derived. Context free grammars are widely used, for example, one could construct a CFG that accepts a language iff it has the proper syntax (as in the case for some programming languages).

#### Example grammars and their languages

* $$ S \rightarrow AA
   \\ A \rightarrow A $$ &nbsp; This CFG is invalid, as there are no terminal symbols, so $L = \emptyset$
* $$ S \rightarrow aSa \mid aBa
   \\ B \rightarrow Bb \mid b $$ &nbsp; For this CFG, $L = \\{a^nb^+a^n,  n \geq 1\\}$. 
* $$ S \rightarrow abS \mid a$$ &nbsp; For this CFG, $L = \\{(ab)^*a\\}$.
* $$ S \rightarrow aSa \mid B
   \\ B \rightarrow bB \mid \epsilon $$ &nbsp; For this CFG, $L = \\{a^nb^*a^n, n \geq 0 \\}$.
* $$ S \rightarrow SS \mid aS \mid b$$ &nbsp; For this CFG, $L = \\{(\Sigma)^*b\\}$.
* $$ S \rightarrow AB \\ A \rightarrow aAa \mid \epsilon
   \\ B \rightarrow bBb \mid \epsilon$$ &nbsp; For this CFG, $L = \\{ (aa)^* (bb)^* \\} $.

#### Constructing a grammar for a language

* $L = \\{ a^n (a \cup b)^n, n \geq 0 \\}$ is defined by the CFG: &nbsp; $$ S \rightarrow aST \mid \epsilon \\ T \rightarrow a \mid b $$
* $L = \\{ w : w = w^R  \\}$  is defined by the CFG: &nbsp; $$ S \rightarrow aSa \mid bSb \mid b \mid a \mid \epsilon $$
* $L = \\{ (a \cup b)^n a (a \cup b)^n, n \geq 0 \\}$  is defined by the CFG: &nbsp; $$ S \rightarrow TST \mid a \\ T \rightarrow a \mid b $$
* $L = \\{ w : w \text{ contains } baa \\}$  is defined by the CFG: &nbsp; $$ S \rightarrow TbaaT \\ T \gets aT \mid bT \mid \epsilon $$
* $L = \\{ a^nb^m, n \geq  m \\}$  is defined by the CFG: &nbsp; $$ S \rightarrow aSb \mid aS \mid \epsilon$$
* $L = \\{ a^nb^m, n >  m \\}$  is defined by the CFG: &nbsp; $$ S \rightarrow aSb \mid aS \mid a$$
* $L = \\{ a^nb^m, 0 \leq m \leq 3n \\}$  is defined by the CFG: &nbsp; $$ S \rightarrow aSBBB \mid \epsilon \\ B \rightarrow b \mid \epsilon$$
* $ L = \\{ w :  w \text{ contains more than 3 } a \\}$


