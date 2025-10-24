Let's talk about Binary Addition. In this note we will unravel addition using binary numbers step by step.

Before we talk about binary addition let's recall of we perform addition of base 10 numbers.

Consider the addition of two base 10 numbers: 

$$
\begin{array}{r@{}r@{}r@{}r}
 &  & 1 & \\[2pt]
 & 4 & 2 & 7 \\[-1pt]
+ & 3 & 5 & 8 \\ \hline
 & 7 & 8 & 5
\end{array}
$$
In the operation shown above, we first start the addition from the rightmost column, which we can also call the ones place or even more aligned with our main goal of binary addition, the column with the least significant digits.

We can see that when we add the rightmost digits and the sum is greater ***(base - 1)*** we leave the sum as it is and carry the 1 to the next column: so if the result of 7 + 8 is 15 we leave 5 as it is and carry the 1 to the next column. We can then add it with the next columns digits. 

This little detour allowed us to observe that to the result of the addition of two digits can be split into two parts the so called ***sum*** part and the ***carry*** part. The carry part will be zero as long as the sum does not exceed the ***(base - 1)***. 

This little insight into the mechanics of how we have always performed addition as children is crucial to build the idea of a binary adder.

Let's now consider adding two binary numbers instead now:

$$
\begin{array}{r}
01100101 \\
+\,10110110 \\ \hline
100011011
\end{array}
$$
As in decimal addition we add two binary numbers column by column starting at the rightmost column.

This addition can go a lot faster if we use an addition table or even better if we have memorized one.

Below given is an addition table for binary addition: 
$$
\begin{array}{|c|c|c|}
\hline
+ & 0 & 1 \\ \hline
0 & 0 & 1 \\ \hline
1 & 1 & 10 \\ \hline
\end{array}
$$
We can modify this table to always use two bit values for consistency: 

$$
\begin{array}{|c|c|c|}
\hline
+ & 0 & 1 \\ \hline
0 & 00 & 01 \\ \hline
1 & 01 & 10 \\ \hline
\end{array}
$$
From this modified table its a bit easier to see the sum and carry separation that we talked about when discussing decimal addition. In each of the results in the preceding table, the right bit is the ***sum*** bit and the left bit is the ***carry*** bit.

Let's split the table into two tables, showing the sum and carry results in separate tables.

**Sum Table**

$$
\begin{array}{|c|c|c|}
\hline
\ Sum & 0 & 1 \\ \hline
0 & 0 & 1 \\ \hline
1 & 1 & 0 \\ \hline
\end{array}
$$
**Carry Table**

$$
\begin{array}{|c|c|c|}
\hline
Carry & 0 & 1 \\ \hline
0 & 0 & 0 \\ \hline
1 & 0 & 1 \\ \hline
\end{array}
$$
It's convenient for use to look at binary addition this way(separating the sum and carry). Building a binary adder requires us to build circuits that perform these operations and working solely in binary simplifies the problem because all the circuit components can be binary digits.

Now let's think about how we can design a circuit that performs addition.

We will first try to design the circuit that computes the ***Carry*** part of the addition.

Looking at the addition table for ***Carry***, we notice that the table looks identical to the **Truth Table** for the AND GATE

Here is the Truth table for the AND gate: 

$$ 
\begin{array}{|c|c|c|}
\hline
AND & 0 & 1 \\ \hline
0 & 0 & 0 \\ \hline
1 & 0 & 1 \\ \hline
\end{array}
$$
Great!, we just use an AND gate to perform the carry computation now we just need to find a way to perform the sum computation and we we will be off to the races.

Let's analyze the ***Sum*** table now.

$$
\begin{array}{|c|c|c|}
\hline
\ Sum & 0 & 1 \\ \hline
0 & 0 & 1 \\ \hline
1 & 1 & 0 \\ \hline
\end{array}
$$
The Sum table looks like the table for the OR gate except for the bottom right cell.
$$
\begin{array}{|c|c|c|}
\hline
\ OR & 0 & 1 \\ \hline
0 & 0 & 1 \\ \hline
1 & 1 & 1 \\ \hline
\end{array}
$$
We also realize that it is similar to the NAND gate table except for the top left cell

$$
\begin{array}{|c|c|c|}
\hline
\ NAND & 0 & 1 \\ \hline
0 & 1 & 1 \\ \hline
1 & 1 & 0 \\ \hline
\end{array}
$$

If only we had a way to combine them to get our desired output, let's try connecting them to the same inputs and compare the results with our desired output.
We will use the following circuit.

![[binary addition 1.png]]


Here is a table summarizing the outputs:

| A In | B In | OR out | NAND out | What we want |
| ---- | ---- | ------ | -------- | ------------ |
| 0    | 0    | 0      | 1        | 0            |
| 0    | 1    | 1      | 1        | 1            |
| 1    | 0    | 1      | 1        | 1            |
| 1    | 1    | 1      | 0        | 0            |
Looking at this table we realize that we only want the output to be one when the output of both the OR gate and AND gate is one, this suggests that these two outputs can be input to an AND gate.

![[binary addition 2.png]]

Here is the table with updated labels. Notice we still only use two inputs and one output.

| A In | B In | OR out | NAND out | AND out |
| ---- | ---- | ------ | -------- | ------- |
| 0    | 0    | 0      | 1        | 0       |
| 0    | 1    | 1      | 1        | 1       |
| 1    | 0    | 1      | 1        | 1       |
| 1    | 1    | 1      | 0        | 0       |

Also worth noting is that the final output is one only when one of the inputs is one, i.e one of the inputs is exclusively one. There is a name for this and that is the XOR gate or the Exclusive OR gate.

XOR gate is represented as follows:

![[binary addition 3.png]]

The truth table for the XOR gate: 

$$
\begin{array}{|c|c|c|}
\hline
\ XOR & 0 & 1 \\ \hline
0 & 0 & 1 \\ \hline
1 & 1 & 0 \\ \hline
\end{array}
$$
Now, that we have all the pieces let's review what we learned so far.

Adding two binary numbers produces two bits: a *sum* bit and a *carry* bit.

$$
\begin{array}{|c|c|c|}
\hline
\ Sum & 0 & 1 \\ \hline
0 & 0 & 1 \\ \hline
1 & 1 & 0 \\ \hline
\end{array}
\quad\quad
\begin{array}{|c|c|c|}
\hline
Carry & 0 & 1 \\ \hline
0 & 0 & 0 \\ \hline
1 & 0 & 1 \\ \hline
\end{array}
$$
These two logic gates can be used to compute them

$$
\begin{array}{|c|c|c|}
\hline
\ XOR & 0 & 1 \\ \hline
0 & 0 & 1 \\ \hline
1 & 1 & 0 \\ \hline
\end{array}
\quad\quad
\begin{array}{|c|c|c|}
\hline
AND & 0 & 1 \\ \hline
0 & 0 & 0 \\ \hline
1 & 0 & 1 \\ \hline
\end{array}
$$
The sum of two binary numbers is given by the XOR gate and the carry is given by the AND gate. We can combine the two gates to add two binary numbers A and B.

![[binary addition 4.png]]

Instead of drawing all these gates we can simply draw a box like this: 

![[binary addition 5.png]]

The box is labeled half adder for a reason. Certainly it adds two binary numbers and gives you a sum and carry bit, but what the adder fails to do is include and add a possible carry from the previous addition.

$$
\begin{array}{r}
1111 \\
+\,1111 \\ \hline
11110
\end{array}
$$
In the above addition we can only use the half adder for the first, rightmost addition. For subsequent addition we really need to add three numbers: the two inputs and the carry from the previous calculation.

Let's think how we can build a circuit that can add two numbers taking into account the carry from the previous addition.

The main problem with the Half Adder is its inability to add the carry from the previous step, so let's introduce another half adder into the circuit using which we can add the carry from the previous step. But wait what do we add the carry to? The ***Sum*** or the ***Carry*** of the current calculation?

Let's understand this with the help of the previous addition example: 

$$
\begin{array}{r}
1111 \\
+\,1111 \\ \hline
11110
\end{array}
$$
When looking at the second column we can see we need to add three 1s: two from the current calculation and one carry from the previous calculation. We can add the carry to either of the 1s or the result of the current computation to the carry either way is fine, we will go with the second approach.

![[binary addition 6.png]]

Let's walk through the addition of the second column of the addition example. First the two ones are added to give a ***Sum*** of 0 and a ***Carry*** of 1. The new ***Sum*** is then added to the previous ***Carry*** which is 1. This produces a result with ***Sum*** 1 and ***Carry*** 0. We can the add the carries together using an OR gate; we don't need another full adder here because both the Carry outs are never 1 together, hence the OR gate is same as the XOR gate here.

Instead of redrawing this whole circuit every time we need to add two numbers we can just the below representation: 

![[binary addition 7.png]]

Now, we can chain 8 of these together to add two 8 bit binary numbers.

Let's build the circuit with all 8 of the full adders chained. When building the circuit the first thing we notice is that the first addition of the first column of digits by the first full adder is different to all other additions, this is because any subsequent additions might include a carry from the previous addition. So, for this first addition we set the ***Carry In*** to 0 and add the numbers in the first column.

For the first column we use a full adder wired this way: 

![[binary addition 8.png]]

Notice the ***Carry In*** is set to ground.

For the subsequent addition we use a full adder wired this way: 

![[binary addition 9.png]]

The carry from the previous addition is passed in to the ***Carry In*** input of this full adder, all subsequent full adders are wired this way.

Finally the last full adder is wired the following way: 

![[binary addition 10.png]]

The last carry out is wired to the ninth bulb which will light up when the result of the addition is greater than 8 bits or digits.

Here is the complete circuit.

![[binary addition 11.png]]

We can draw this more concisely like this: 

![[binary addition 12.png]]

We can even chain two these together to perform 16 bit addition like this: 

![[binary addition 13.png]]