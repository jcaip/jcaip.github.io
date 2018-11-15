---
title: Languages and Automata
tags: theory

---
Some notes for CS181. This is about context-free grammars, pushdown automata, the pumping lemma for context-free grammars, and closure properties of context-free languages.

<!--more-->
## Context-Free Grammars

A context-free grammar is represented by a tuple $( V, \Sigma, R, S)$ where
* $V$ is the set of all variables
* $\Sigma$ is the alphabet
* $R$ are all the derivation rules (i.e. $S \rightarrow aSb \mid \epsilon$)
* $S$ is the start variable

At each step in time, you can replace one of the variables with one of it's derivations.
The language of a grammar is the set of all strings that can be derived. Context free grammars are widely used, for example, one could construct a CFG that accepts a language iff it has the proper syntax (as in the case for some programming languages).

#### Example grammars and their languages

1. $$ S \rightarrow AA
   \\ A \rightarrow A $$ &nbsp; This CFG is invalid, as there are no terminal symbols, so $L = \emptyset$

2. $$ S \rightarrow aSa \mid aBa
   \\ B \rightarrow Bb \mid b $$ &nbsp; For this CFG, $L = \\{a^nb^+a^n,  n \geq 1\\}$. 

3. $$ S \rightarrow abS \mid a$$ &nbsp; For this CFG, $L = \\{(ab)^*a\\}$.

4. $$ S \rightarrow aSa \mid B
   \\ B \rightarrow bB \mid \epsilon $$ &nbsp; For this CFG, $L = \\{a^nb^*a^n, n \geq 0 \\}$.

5. $$ S \rightarrow SS \mid aS \mid b$$ &nbsp; For this CFG, $L = \\{(\Sigma)^*b\\}$.

6. $$ S \rightarrow AB \\ A \rightarrow aAa \mid \epsilon
   \\ B \rightarrow bBb \mid \epsilon$$ &nbsp; For this CFG, $L = \\{ (aa)^* (bb)^* \\} $.

#### Constructing a grammar for a language

1. $L = \\{ a^n (a \cup b)^n, n \geq 0 \\}$ is defined by the CFG: &nbsp; $$ S \rightarrow aST \mid \epsilon \\ T \rightarrow a \mid b $$

2. $L = \\{ w : w = w^R  \\}$  is defined by the CFG: &nbsp; $$ S \rightarrow aSa \mid bSb \mid b \mid a \mid \epsilon $$

3. $L = \\{ (a \cup b)^n a (a \cup b)^n, n \geq 0 \\}$  is defined by the CFG: &nbsp; $$ S \rightarrow TST \mid a \\ T \rightarrow a \mid b $$

4. $L = \\{ w : w \text{ contains } baa \\}$  is defined by the CFG: &nbsp; $$ S \rightarrow TbaaT \\ T \gets aT \mid bT \mid \epsilon $$

5. $L = \\{ a^nb^m, n \geq  m \\}$  is defined by the CFG: &nbsp; $$ S \rightarrow aSb \mid aS \mid \epsilon$$

6. $L = \\{ a^nb^m, n >  m \\}$  is defined by the CFG: &nbsp; $$ S \rightarrow aSb \mid aS \mid a$$

7. $L = \\{ a^nb^m, 0 \leq m \leq 3n \\}$  is defined by the CFG: &nbsp; $$ S \rightarrow aSBBB \mid \epsilon \\ B \rightarrow b \mid \epsilon$$

8. $L = \\{ w :  w \text{ contains more than 3 } a \\}$ is defined by the CFG:  &nbsp; $$ S \rightarrow TaTaTaT \\T \rightarrow aT \mid bT \mid \epsilon$$

9. $L = \\{ a^nb^nb^ma^m, n,m \geq 0 \\}$ is defined by the CFG: &nbsp; $$ S \rightarrow UR \\ U \rightarrow aUb \mid \epsilon \\ R \rightarrow bRa \mid \epsilon$$

10. $L = \\{ \Sigma (a \cup b)^n a (a \cup b )^n, n \geq 0 \\}$ is defined by the CFG: &nbsp; $$ S \gets TR \\ T \gets a \mid b \\ R \gets TRT \mid a $$


Key things to note here: try to break down the CFG into smaller, simpler parts and then try to compose these parts in order to get a solution for the language. Some key building blocks to keep in mind are examples 3 and 5.

### Ambiguity

A grammar is abiguous if some string has multiple distinct parse trees. A common source of abiguity is empty strings, or $\epsilon$.

For example, the CFG $$ S \rightarrow R \mid T \\ R \rightarrow aRb \mid \epsilon \\ T \rightarrow aT \mid \epsilon $$ is ambiguous, as the string $\epsilon$ can be parsed either using $T$ or $R$.

#### Converting an ambiguous grammar into a non-ambiguous grammar

1. $$ S \rightarrow ABAB \\ A \rightarrow aA \mid \epsilon \\ B \rightarrow bB \mid \epsilon$$ &nbsp; is ambiguous for the string $ab$.

    A non-ambiguous grammar is given by &nbsp; $$ S \rightarrow ABAB \mid BAB \mid ABA \mid BA \mid AB \mid A \mid B \mid \epsilon \\ A \rightarrow aA \mid a \\ B \rightarrow bB \mid b$$

2. $$ S \rightarrow aSb \mid aaSb \mid \epsilon$$ &nbsp; is ambiguous for the string $aaabb$.

    A non-ambiguous grammar is given by &nbsp; $$ S \rightarrow aSb \mid R \\ R \rightarrow aaRb \mid \epsilon$$. This imposes an ordering so we expand $ab$ before we expand using $aab$.

3. $$ P \rightarrow PP \mid aPb \mid ab$$ &nbsp; is ambiguous for the string $ababab$.

    A non-ambiguous grammar is given by &nbsp; $$P \rightarrow ab \mid aPb \mid abP \mid aPbP $$. **Note:** This is the language of all properly nested parenthesis. We make it non-ambiguous by anchoring on open parenthesis.


#### Designing unambiguous grammars

1. Design an unambiguous grammar for $L = \\{w : w \text{ has at least as many } a \text{ as } b \\}$. We can use the unambiguous language designed in 3, but just consider the other case where the parenthesis may be switched.
    $$ S \rightarrow S^{'}\mid S^{''} \mid \epsilon \\  S^{'} \rightarrow P S^{''} \mid P \\ S^{''} \rightarrow Q S^{'} \mid Q \\ P \rightarrow ab \mid aPb \mid abP \mid aPbP \\ Q \rightarrow ba \mid bQa \mid baQ \mid bQaQ $$


## Pushdown Automata

Pushdown automata can be thought of as an extension to normal NFA/DFA. However, the automata now has access to a stack (FIFO), and each transition is of the from $\alpha, \beta \rightarrow \gamma$. 
* $\alpha$ is character that needs to be read to make the transition. 
* $\beta$ is character that needs to be popped from the stack ($\epsilon$ means don't pop anything). 
* $\gamma$ is character that needs to be pushed onto the stack ($\epsilon$ means don't push anything). 

A pushdown automata is then represented by the tuple $(Q, \Sigma, \Gamma, \delta, q_0, F)$ where 
* $Q$ is the set of all states
* $\Sigma$ is the input alphabet
* $\Gamma$ is the stack alphabet
* $\delta$ is the transition function. $\delta : Q \times (\Sigma \cup \\{\epsilon\\}) \times (\Gamma \cup \\{\epsilon \\}) \rightarrow Q$
* $q_0$ is the start state $q_0 \in Q$
* $F$ is the set of accept states $F \subseteq Q$

Every language that has a context-free grammar can be written as a PDA, and vice versa. 

To do this, given a CFG described by $(V, \Sigma, S, R)$, we can construct a corresponding PDA $(\\{q_{start}, q_{loop}, q_{accept}, \\} \cup E, \Sigma, \Sigma \cup V \cup \\{ \epsilon \\}, \delta, q_{start}, \\{q_{accept}\\})$ where $\delta$ is defined so that. 

* $\delta(q_{start}, \epsilon, \epsilon) = (q_{loop}, S \perp)$
* $\forall \text{ variables } S \text{ and their respective derivation } w . \delta(q_{loop}, \epsilon, S) = (q_{loop}, w)$ 
* $\forall \text{ terminals } t . \delta(q_{loop}, t, t) = (q_{loop}, \epsilon)$ 
* $\delta(q_{loop}, \epsilon, \perp) = (q_{accept}, \epsilon)$

![comp](/images/lang/pda_uncomp.png)

To go from a PDA to a grammar, we first have to clean up the PDA. We do this by first enforcing a unique accept state, by adding $\epsilon$ transitions from each accept state in the PDA to a new accept state outside of the PDA. 

Next, we ensure that the PDA empties it's stack before accepting, by adding in $\perp$ to $\Gamma$ and add in a state with a loop that will dump the stack before accepting.

Finally we modify all the $\delta$ transitions so that each transition is either a push or a pop. This is easy to do - if our $\delta$ transition is all $\epsilon$ we can just expand it, otherwise we can split them.

If $L$ is recognized by a PDA given by $(Q, \Sigma, \Gamma, \delta, q_0, F)$ then $L$ is a CFG with the following rules: 

$$
    \forall_{q \in Q} . V_{q,q} \rightarrow \epsilon \\
    \forall_{p, q, r \in Q} . V_{p, q} \rightarrow V_{p, r} V_{r, q} \\
    \forall_{p, q, r \in Q} \forall_{\sigma, \tau \in \Sigma_\epsilon } \forall_{\gamma \in \Gamma} \mid \delta(p, \sigma, \epsilon) \ni (r, \gamma), \delta(s, \tau, \gamma) \ni (q, \epsilon) . V_{p, q} \rightarrow \sigma V_{r, s} \tau
$$

More specifically, for $p, q \in Q$ $L_{p,q} = \\{ w : w \text{ can take the PDA from } p \text{ w/ empty stack to } q \text{ w/ empty stack }\\}$. Thus $L_{PDA} = L_{q_{start} q_{final}}$

With this in mind, we can say that language $L$ is context-free if it has a conteset-free grammar of a PDA. It's also easy to see that every regular language has a context-free grammar. 

## Pumping Lemma for Context-Free Languages

Context-free languages have a pumping lemma, similar to regular languages.

$$ L \text{ is a CFL } \implies \exists p . \forall s \in L \text{ s.t. } \vert{s}\vert \geq p . \exists u v x y z \text{ s.t. } s = uvxyz, \vert{vxy}\vert \leq p, \vert{v}\vert + \vert{y}\vert \neq 0 . \forall i . uv^ixy^iz \in L$$

To prove that a language is **not** context free, we will use the contrapositive of the pumping lemma.

$$\forall p . \exists s \in L \text{ s.t. } \vert{s}\vert \geq p . \forall u v x y z \text{ s.t. } s = uvxyz, \vert{vxy}\vert \leq p, \vert{v}\vert + \vert{y}\vert \neq 0 . \exists i . uv^ixy^iz \notin L  \implies  L \text{ is not a CFL} $$

### Examples

1. $L = \\{ a^nb^nc^n : n \geq 0\\}$

    Let $p$ be arbitrary, and $s = a^pb^pc^p$. For any valid decomposition, $uxz \notin L$, as we have to be removing a character, but it is impossible for us to remove one of each, as $\vert{vxy}\vert \leq p$, so it can span at most one transition, either from $a$ to $b$ or $b$ to $c$. Therefore $L$ is not a CFL.

### Proof of Pumping Lemma

TODO: This is a bit involved, so skipping this for now. 

## Closure Properties

* CFLs are closed under union.
* CFLs are closed under concatenation.
* CFLs are **not** closed under intersection.
* CFLS are **not** closed under complement.
* CFLs are closed under Kleene star.
* CFLs are closed under reversal.
