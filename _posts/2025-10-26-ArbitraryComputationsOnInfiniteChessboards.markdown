---
layout: post
title:  "(WIP) Arbitrary Computations on Infinite Chessboards"
date:   2025-10-26 1:51:00 -0400
categories: computation recreational
tags: [Theoretical Computer Science]
math: true
---

# Arbitrary Computation on Infinite Chessboards
Recently, I watched a YouTube video by [Wrath of Math](https://youtu.be/0_Qe_0aj4eEM?si=HFrCcPmOmu88-fIP), entitled "Why You Can't Bring Checkerboards to Math Exams". In this short, Wrath of Math demonstrates a binary multiplication and division algorithm for 8-bit integers. This algorithm is just one example of computations on unconventional items. While multiplication and division are interesting as they are, one may ask what the natural limits of computation are on a chessboard. Can one compute *anything* on a chessboard? For the infinite case, the answer may not be what you expect. 

## Languages and the Limit of the 8x8 Chessboard

When viewing the chessboard at face value, it is not immediately clear what can be computed on it. In the standard game of chess, we only have finite types of pieces (six black and six white pieces), and only 64 squares to place them on. While the usual interpretation of computations is via the route of algorithms, as in a set of steps to achieve solutions to problems, there are no definitive ways to construct standardized algorithms on the chessboard. To contextualize the chessboard more abstractly, we introduce the concept of languages: 

**Definition 1:** An alphabet is a finite set of charecters $\Sigma$.

**Definition 2:** A language is a set of finite length strings from an alphbet $\Sigma$. That is: $L \subseteq \Sigma^*$

In layman's terms, a language is simply a set of strings. As an example, a hypothetical language on $\{0,1\}$ could be:

$$L = \{0, 1, 01, 10101, 1011, 11111\}$$

Languages, while simple and unassuming at first, actually have one abillity that makes them incredibly useful to contextualize computation: we can use them to encode problems!

If we had a problem $P$ and a set of valid solutions $x_1,x_2,...,x_n$, as long as we can encode these solutions (by a function $e$) as a set of strings over some alphabet $\Sigma$, $\{e(x_1), e(x_2), ..., e(x_n)\}$ would be a valid language. Specifically, this language is encoding the "yes" solutions to $P$. This leads us to a direct way to consider the limits of computations on $P$: find what kinds of languages can and can not be encoded onto a chessboard. 

To do so, we denote $C$ to be the set of possible placements of an arbitrary number of chess pieces on a chessboard, regardless of the typical rules of chess. The size of this set is a whopping $13^{64}$: each square can have one of six white or black pieces, or none, on all 64 squares. We define the **transition function** $\delta: C \rightarrow C$ to be the set of possible moves from any one state to any other. It is important to note here that the term "transition function" is a bit of a misnomer here. Typically, there are many possible moves from any state to any other, making this a relation by technicality. For now, we will disregard exactly which moves are considered legal, as it does not alter the theoretical limits of language recognition on our chessboard.  We now will proceed to show that $C$ can not possibly be Turing-complete (and thus can not compute anything): 

Since the number of states $C$ can recognize is $13^{64}$, this implies that any sequence of moves can only take on finitely many unique states—allbeit a truly massive amount. By the pidgeonhole principle, if our computation path is longer than $13^{64}$ moves, we must revisit some state more than once. 

This dependency on cyclical structure after a finite number of computational moves disallows the chessboard from computing arbitrarily without repitions. In computer scientific terms, the chessboard behaves equivalently **finite state machine**. This means that at most, the chessboard can recognize a subset of the [**regular languages**](https://en.wikipedia.org/wiki/Regular_language). If a computation requires an infinite number of computational steps not bounded by a single cycle, the chessboard will be unable to recognize it. Such a language could be $L = \{a^nb^m\ | n = m + 3\}$, where no single pre-determined cycle can account for the interdependence of the powers of $a$ and $b$.

The logic of our argument mirrors the [**Pumping Lemma**](https://en.wikipedia.org/wiki/Pumping_lemma_for_regular_languages#:~:text=Specifically,%20the%20pumping%20lemma%20says,is%20known%20as%20%22pumping%22.); a famous theorem that provides a bound on whats possible to recognize on a finite state machine. In fact, no finite chessboard will suffice here as while the state space $C$ can grow arbitrarily large, it is never infinite, meaning we can always find arbitrarily long computation paths that force repetitions in the way that we did with the 8x8 chessboard.

So let's pump the gas a little and move to the infinite chessboard!

## the Infinite Chessboard and the Turing Machine

We now move to the case where our chessboard is not 8x8, but rather infinite in the horizontal direction as well as vertically from row 1. Each possible arrangement of pieces now represents one of *countably* many configurations, escaping the confines of language recognition on finite-sized chessboards. 

With this in mind, we can begin to think about constructing a method of computation similar to that of the Turing Machine on our chessboard. To better understand how, let us first visit the formal definition of a Turing Machine:

**Definition 3:** A [Turing Machine](https://en.wikipedia.org/wiki/Turing_machine) is a tuple $M = \langle Q, \Gamma, b, \Sigma, \delta, q_0, F \rangle$ where:

 1. $\Gamma$ is a finite, non-empty set of symbols used as an alphabet on the tape;
 2.  $b \in \Gamma$ is the **blank symbol** (the only symbol allowed to occur on the tape infinitely often and is the default symbol on the tape.)
3. $\Sigma \subseteq \Gamma \setminus \{ b \}$ is the set of **tape symbols** that can appear on the tape initially (at time 0);
 4.  $Q$ is a finite, non-empty set of **states** the machine can be in;
5. $q_0 \in Q$ is the **starting state** of the turing machine;
 6. $F \subseteq Q$ is the set of **accepting states**;
 7. $\delta : (Q \setminus F) \times \Gamma \to Q \times \Gamma \times \{L, R\}$ is a partial function, where $L$ and $R$ denote left and right shifts respectively.

While it may seem unintuitive at first, we are simply defining a set of transitions $\delta$ defining the movements of a "head" over an infinite tape. The tape is initalized with some input string over $\Sigma$ and initialize the tape to be in the state $q_0$. which is an input string over $\delta$ specifies, based upon the current state and symbol on the tape, a new state for the machine to be in, a symbol to overwrite the current state with, and whether or not to move the head left or right. 

The machine then follows the deterministic set of moves laid out by $\delta$ and if the machine ends on an accepting state $F$ in finite time, the string is **accepted**. We define the language $L(M)$ defined by the Turing Machine M to be the set of input strings that are accepted by the Turing Machine. If the Turing machine ends on a state not in $F$ and has no defined moves left, the machine halts and that particular input string is **rejected**. 

To construct our new model of computation, we take heavy inspiration from the original Turing Machine. Our goal is to construct a set of moves on the chessboard that effectively mimics the movements of the Turing Machine. 

It should be noted here that our initial alphabet and tape alphabet $\Sigma$ and $\Gamma$ will both be $\{0, 1\}$. We define 0 to be the presence of a pawn on a given square and 1 to be the lack of a pawn on a square. we can define our input string and tape, and place them on row 4. The string input exists on all columns before e, and the write tape exists on all columns after e. Nothing exists on row e, except for the starting position of our "head", which will be the knight on e2. For the sake of brevity, we refer to row 4 as the "memory row." 

![A pgoto depicting rows of pawns from row 5 onwards to infinity. Black pawns to the left and white pawns to the right. On row four, there is a mix of pawns and no pawns. The presence of no pawn details a logical one, while a pawn on the square represents a logical 0.](https://raw.githubusercontent.com/linuxwire-blog/blog/refs/heads/main/_assets/images/chessboard/Read%20Tape.png)

The head is able to make 3-move sequences to move exactly one square to the left or right, positioning it to read and write for arbitrary squares on the memory row:

![GIF of the read and 1-write head moving 1 square to the right.](https://raw.githubusercontent.com/linuxwire-blog/blog/main/_assets/images/chessboard/ApronusDiagram1761498967.gif)

We will define $M_{d2\rightarrow c2} K$ to denote the movement of the head from the square d2 to c2, respectively. A read by the knight can then be described as an attempted capture on a square on memory row. If the capture is successful, the machine reads a 0, and if the capture is unsuccessful, the knight reads a 1. After an attempted read, the knight will always make one move back to its starting position. We will denote a read on cell $\alpha$ on memory row as $O_\alpha$. If a knight reads a 0, the piece will be destroyed, leaving a 1 in its place. Thus, to preserve the information on the tape, after the knight returns to its starting position, all the pawns in the succeeding rows must move down exactly one square to reset the machine. 

![The set of moves that denote a read of 0 on column d of the input tape.](https://raw.githubusercontent.com/linuxwire-blog/blog/main/_assets/images/chessboard/ApronusDiagram1761499727.gif)

To write a one on column $\beta$, the knight captures on that square. But to write a 0, all the pawns on a given column need to move down exactly one square to write a 1. Thus, the task of writing is divided into two different structures, unlike the typical Turing machine. 

The only question that remains is how to show that this chessboard computer is logically equivalent to a Turing machine.
## Turing Machine Isomorphisms

## Conclusions

This exercise demonstrates that while the Turing Machine itself may seem abstract and byzantine, it can be emulated readily in a myriad of different ways, even on a chessboard. 
