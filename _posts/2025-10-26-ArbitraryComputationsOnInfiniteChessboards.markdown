---
layout: post
title:  "(WIP) Arbitrary Computations on Infinite Chessboards"
date:   2025-10-26 1:51:00 -0400
categories: computation recreational
tags: [theoretical computer science]
math: true
---

# Arbitrary Computation on Infinite Chessboards
Recently, I watched a YouTube video by [Wrath of Math](https://youtu.be/0_Qe_0aj4eEM?si=HFrCcPmOmu88-fIP), entitled "Why You Can't Bring Checkerboards to Math Exams". Here, he demonstrates a binary multiplication and division algorithm for 8-bit integers. While multiplication and division are interesting as they are, I immediately wondered what if I wanted to compute *anything* on a chessboard, and this sent me down a rabbit hole.

## Languages and the Limit of the 8x8 Chessboard
Spurred on by this video, the first natural question to ask is, what are the limits of computation for our standard chessboard? Intuitively, we only have finite types of pieces (six black and six white pieces), and only 64 squares to place them on. While the usual interpretation of computations is via the route of algorithms, as in a set of steps to achieve solutions to problems, we can use an alternative encoding (languages) to turn our contemplation into a simple counting problem. 

Languages are simply sets of strings that we derive from an alphabet $\Sigma$. Formally, we say that a language $L \subseteq \Sigma^*$. For example a language on $\{0,1\}$ could be:

$$L = \{0, 1, 01, 10101, 1011, 11111\}$$

These sets can also be infinite, containing the same number of elements as the natural numbers $\mathbb{N}$. Languages can also encode any computational problem that has a yes-or-no answer, also known as a decision problem. This is done by encoding mathematical structures, such as sets or graphs, as strings, and putting them in a language if they are a *yes* anwser to the given problem. Some problems also have more than one, or even an infinite number of correct solutions, providing us with an use of infinite languages in describing problems. 

We can model our chessboard as a model over a finite set $C$ of chessboard configurations. Each element of $C$ is then one of the $13^{64}$ possible placements of chess pieces on a chessboard. We can describe the function of possible moves from one state to any other as $\delta: C \rightarrow C$. Here, we will ignore exactly which moves are considered legal, as it is irrelevant to our argument. Then, since $C$ is finite, there are only a finite number of possible moves from any state $ c^* \in C$ to any other. Thus, if there exists a computation path on our chessboard greater than $13^{64}$, it must go through some cycle. Thus, an 8x8 chessboard can recognize some subset of the regular languages. Our argument here mirrors the [Pumping Lemma](https://en.wikipedia.org/wiki/Pumping_lemma_for_regular_languages#:~:text=Specifically,%20the%20pumping%20lemma%20says,is%20known%20as%20%22pumping%22.); a famous theorem in the theory of regular languages. A universal computer recognizes all languages and not just regular languages, meaning that it is impossible to consturct a universal computer on an 8x8 chessboard. In fact, no finite chessboard will suffice here as while the state space $C$ can grow arbitrarily large, it is never infinite. 

So let's pump the gas a little and move to the infinite chessboard!

## the Infinite Chessboard and the Turing Machine

Here, the immediate issue of finite states immediately disappears, since there are, well, countably many of them. 


To construct our new model of computation, we take heavy inspiration from the original Turing Machine. We place the active read and write tapes on row 4. The read tape exists on all columns before e, and the write tape exists on all columns after e. Nothing exists on row e, except for the logical "1" write-head, which starts on square e2. For the sake of brevity, we refer to row 4 as the "memory row." 

![A pgoto depicting rows of pawns from row 5 onwards to infinity. Black pawns to the left and white pawns to the right. On row four, there is a mix of pawns and no pawns. The presence of no pawn details a logical one, while a pawn on the square represents a logical 0.](https://raw.githubusercontent.com/linuxwire-blog/blog/refs/heads/main/_assets/images/chessboard/Read%20Tape.png)

The head is able to make 3-move sequences to move exactly one square to the left or right, positioning it to read and write for arbitrary squares on the memory row:

![GIF of the read and 1-write head moving 1 square to the right.](https://raw.githubusercontent.com/linuxwire-blog/blog/main/_assets/images/chessboard/ApronusDiagram1761498967.gif)

We will define $M_{d2\rightarrow c2} K$ to denote the movement of the head from the square d2 to c2, respectively. A read by the knight can then be described as an attempted capture on a square on memory row. If the capture is successful, the machine reads a 0, and if the capture is unsuccessful, the knight reads a 1. After an attempted read, the knight will always make one move back to its starting position. We will denote a read on cell $\alpha$ on memory row as $O_\alpha$. If a knight reads a 0, the piece will be destroyed, leaving a 1 in its place. Thus, to preserve the information on the tape, after the knight returns to its starting position, all the pawns in the succeeding rows must move down exactly one square to reset the machine. 

![The set of moves that denote a read of 0 on column d of the input tape.](https://raw.githubusercontent.com/linuxwire-blog/blog/main/_assets/images/chessboard/ApronusDiagram1761499727.gif)

To write a one on column $\beta$, the knight captures on that square. But to write a 0, all the pawns on a given column need to move down exactly one square to write a 1. Thus, the task of writing is divided into two different structures, unlike the typical Turing machine. 

The only question that remains is how to show that this chessboard computer is logically equivalent to a Turing machine.
## Turing Machine Isomorphisms

## Conclusions
