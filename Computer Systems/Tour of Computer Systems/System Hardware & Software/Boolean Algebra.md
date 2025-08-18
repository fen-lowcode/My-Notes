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
==The Boolean operation ~ corresponds to the logical operation not, denoted by the symbol ¬.== That is, we say that ¬P is true when P is not true, and vice versa. Correspondingly, ~p equals 1 when p equals 0, and vice versa. 

==Boolean operation & corresponds to the logical operation and, denoted==
==by the symbol ∧.== We say that P ∧ Q holds when both P is true and Q is true.
Correspondingly, p & q equals 1 only when p = 1 and q = 1. 

==Boolean operation | corresponds to the logical operation or, denoted by the symbol ∨.== We say that P ∨ Q holds when either P is true or Q is true. Correspondingly, p | q equals 1 when either p = 1 or q = 1. 

==Boolean operation ^ corresponds to the logical operation exclusive-or, denoted by the symbol ⊕.== We say that P ⊕ Q holds when either P is true or Q is true, but not both. Correspondingly, p ^ q equals 1 when either p = 1 and q = 0, or p = 0 and q = 1.

* * * 

>[!History]

Claude Shannon (1916–2001), who later founded the field of information
theory, first made the connection between Boolean algebra and digital logic. In
his 1937 master’s thesis, he showed that Boolean algebra could be applied to the
design and analysis of networks of electromechanical relays. Although computer
technology has advanced considerably since, Boolean algebra still plays a central
role in the design and analysis of digital systems.

* * * 

We can extend the four Boolean operations to also operate on bit vectors,
strings of zeros and ones of some fixed length w. We define the operations over bit
vectors according to their applications to the matching elements of the arguments.
Let a and b denote the bit vectors

$[aw−1, aw−2 , . . . , a0] and [bw−1, bw−2 , . . . , b0],$

respectively. We define a & b to also be a bit vector of length w, where the ith
element equals ai & bi , for 0 ≤ i < w. The operations |, ^, and ~ are extended to
bit vectors in a similar fashion.

As examples, consider the case where w = 4, and with arguments 

$a = [0110]$  and  $b = [1100]$

Then the four operations 

$a & b, a | b, a ^ b,$ and $~b$ yield

![](boolean-algebra-bitwise-example.png)

---

#### Logical Operations in C

C also provides a set of logical operators ||, &&, and !, which correspond to the
or, and, and not operations of logic. These can easily be confused with the bit-
level operations, but their behavior is quite different. The logical operations treat
any nonzero argument as representing true and argument 0 as representing false.
They return either 1 or 0, indicating a result of either true or false, respectively.

---

#### More on Boolean Algebra and Boolean Rings

The Boolean operations |, &, and ~ operating on bit vectors of length w form a Boolean algebra, for any integer w > 0. The simplest is the case where w = 1 and there are just two elements, but for the more general case there are 2w bit vectors of length w. 

Boolean algebra has many of the same properties as arithmetic over integers. For example, just as multiplication distributes over addition, written a . (b + c) = (a . b) + (a . c), Boolean operation & distributes over |, written a & (b | c) = (a & b) | (a & c). In addition, however. Boolean operation | distributes over &, and so we can write a | (b & c) = (a | b) & (a | c), whereas we cannot say that a + (b . c) = (a + b) . (a + c) holds for all integers. 

When we consider operations ^, &, and ~ operating on bit vectors of length w, we get a different mathematical form, known as a Boolean ring. Boolean rings have many properties in common with integer arithmetic. 

For example, one property of integer arithmetic is that every value x has an additive inverse −x, such that x + −x = 0. A similar property holds for Boolean rings, where ^ is the “addition” operation, but in this case each element is its own additive inverse. That is, a ^ a = 0 for any value a, where we use 0 here to represent a bit vector of all zeros. 

We can see this holds for single bits, since 0 ^ 0 = 1 ^ 1 = 0, and it extends to bit vectors as well. This property holds even when we rearrange terms and combine them in a different order, and so (a ^ b) ^ a = b.

---