#math #computer-system 

#### Introduction to Boolean Algebra

Since binary values are at the core of how computers encode, store, and 
manipulate information, a rich body of mathematical knowledge has evolved around the study of the values 0 and 1. T

his started with the work of George Boole (1815–1864) around 1850 and thus is known as Boolean algebra. Boole observed that by encoding logic values true and false as binary values 1 and 0, 

he could formulate an algebra that captures the basic principles of logical reasoning.
The simplest Boolean algebra is defined over the two-element set {0, 1}.

The picture below `Figure 2.7` defines several operations in this algebra. Our symbols for representing these operations are chosen to match those used by the C bit-level operations, as will be discussed later.


![](boolean-algebra-example.png)

>[!important]

* * * 
The Boolean operation ~ corresponds to the logical operation not, denoted by the symbol ¬. That is, we say that ¬P is true when P is not true, and vice versa. Correspondingly, ~p equals 1 when p equals 0, and vice versa. 

Boolean operation & corresponds to the logical operation and, denoted
by the symbol ∧. We say that P ∧ Q holds when both P is true and Q is true.
Correspondingly, p & q equals 1 only when p = 1 and q = 1. 

Boolean operation | corresponds to the logical operation or, denoted by the symbol ∨. We say that P ∨ Q holds when either P is true or Q is true. Correspondingly, p | q equals 1 when either p = 1 or q = 1. 

Boolean operation ^ corresponds to the logical operation exclusive-or, denoted by the symbol ⊕. We say that P ⊕ Q holds when either P is true or Q is true, but not both. Correspondingly, p ^ q equals 1 when either p = 1 and q = 0, or p = 0 and q = 1.

* * * 

>[!History]

Claude Shannon (1916–2001), who later founded the field of information
theory, first made the connection between Boolean algebra and digital logic. In
his 1937 master’s thesis, he showed that Boolean algebra could be applied to the
design and analysis of networks of electromechanical relays. Although computer
technology has advanced considerably since, Boolean algebra still plays a central
role in the design and analysis of digital systems.

* * * 

