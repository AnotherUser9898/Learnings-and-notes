Now, that we know how binary addition works let's tackle binary subtraction in this note.

Binary subtraction and even regular decimal subtraction, unlike binary addition (or regular addition) has quite a messy mechanism with lots of back and forth with borrows. This makes it harder to implement it with an electrical circuit.

Let's first recall how we perform subtraction of two decimal numbers: 

$$
\begin{array}{r@{}r@{}r@{}r}
 & 2 & 5 & 3 \\[-1pt]
- & 1 & 7 & 6 \\ \hline
 & ? & ? & ?
\end{array}
$$
Let's walk through the process of performing this subtraction. We start by performing subtraction on the rightmost column, here we find that 3 is smaller than 6 so we borrow a 1 from 5 to make it 13 and then subtract 13 from 6 which gives us 7. Now, let's go with the second column, here we must remember that due to the borrow 5 is actually 4 and we want to subtract 4 from 7, 4 is smaller than 7 so we borrow a 1 to make it 14 and then subtract 7 from it get the result 7. Now, with the third column where we remember the 2 is actually 1 due to the borrow, we subtract 1 from 1 which gives use zero. So the final result is: 

$$
\begin{array}{r@{}r@{}r@{}r}
 & 2 & 5 & 3 \\[-1pt]
- & 1 & 7 & 6 \\ \hline
 & 0 & 7 & 7
\end{array}
$$
This messy back-and-forth is very difficult to construct with electrical circuits. The primary problem here is the borrow mechanism. We need a way to subtract without borrowing.

Before we do that we need concrete names for the numbers in the subtraction: these will be the ***Subtrahend*** and the ***Minuend*** and the result will be the ***Difference***. The ***Subtrahend*** will be subtracted from the ***Minuend***.

$$
\begin{array}{r@{}r@{}r@{}r}
 &Minuend \\[-1pt]
- & Subtrahend \\ \hline
 & Difference
\end{array}
$$

To subtract without borrowing first subtract the ***Subtrahend*** not from ***Minuend*** but from 999.

$$
\begin{array}{r@{}r@{}r@{}r}
 & 9 & 9 & 9 \\[-1pt]
- & 1 & 7 & 6 \\ \hline
 & 8 & 2 & 3
\end{array}
$$

Subtracting from 999 ensures that we don't need to borrow. We are subtracting from 999 because we are a subtracting a 3 digit number, if we were subtracting a four digit number we would subtract from 9999. Subtracting from a string on 9s is called the *9s Compliment*. The 9s compliment of 176 is 823 and it also works in reverse the 9s compliment of 823 is 176. We will never need to borrow when computing the 9s compliment for reasons that should be obvious now.

After calculating the 9s compliment we add it to the Minuend.

$$
\begin{array}{r@{}r@{}r@{}r}
 & 2 & 5 & 3 \\[-1pt]
+ & 8 & 2 & 3 \\ \hline
 1 & 0 & 7 & 6
\end{array}
$$

If the Minuend is greater than the Subtrahend then the result of this operation will be greater than equal to 1000.

Now, we have the result 1076. To get the final Difference we can add 1 to the result and subtract 1000 from the result.

$$
\begin{array}{r@{}r@{}r@{}r}
 1 & 0 & 7 & 6 \\[-1pt]
+ &  &  & 1 \\ \hline
 1 & 0 & 7 & 7
\end{array}
$$

$$
\begin{array}{r@{}r@{}r@{}r}
 1 & 0 & 7 & 7 \\[-1pt]
- 1 & 0 & 0 & 0 \\ \hline
  &  & 7 & 7
\end{array}
$$

The final answer is 77 and we didn't need to borrow even once.

Now, the most important question is why does this work, how can we prove that result is the same as that of the regular method with method with borrowing.

Let's think about it. This was our original expression.

$$ 253 - 176 $$
We can add and subtract 1000 to this

$$ 253 - 176 + 1000 - 1000 $$
Rearranging the terms we get.
$$ = (1 + 999 - 176 ) + 253 - 1000 $$

$$ = (999 - 176 ) + 253 + 1 - 1000 $$

$$ = 823 + 253 + 1 - 1000 $$
$$ = 77 $$
Since we did not change the original expression, what we did to subtract without borrowing is perfectly valid and produces the expected and correct answer.

Now, let's do the subtraction in reverse.

$$
\begin{array}{r@{}r@{}r@{}r}
 & 1 & 7 & 6 \\[-1pt]
- & 2 & 5 & 3 \\ \hline
 & ? & ? & ?
\end{array}
$$
Normally you would look at this and say, "Hmmm. I see that the subtrahend is larger than the minuend, so I have to switch the two numbers around, perform the subtraction and remember that the result is actually a negative number". You might be able to switch the numbers in your head and write the answer this way.

$$
\begin{array}{r@{}r@{}r@{}r}
 & 1 & 7 & 6 \\[-1pt]
- & 2 & 5 & 3 \\ \hline
 & 0 & 7 & 7 \\ \\
\end{array}
$$
Now, let's try to perform this calculation the same way we did the calculation before. First, we subtract the ***Subtrahend*** from 999 to get the 9s compliment.


$$
\begin{array}{r@{}r@{}r@{}r}
 & 9 & 9 & 9 \\[-1pt]
- & 2 & 5 & 3 \\ \hline
 & 7 & 4 & 6 \\ \\
\end{array}
$$

We then add this difference to the ***Minuend***.

$$
\begin{array}{r@{}r@{}r@{}r}
 & 1 & 7 & 6 \\[-1pt]
+ & 7 & 4 & 6 \\ \hline
 & 9 & 2 & 2 \\ \\
\end{array}
$$

Now, at this point in the earlier problem we were able to add 1 to the sum and subtract a 1000 to get the final answer. That approach however, is not possible here since subtracting 1000 from 923 is just subtracting 923 from 1000 with a negative sign, this cannot be done without borrowing since due to the ***Minuend*** being larger than the ***Subtrahend***  the final sum will always be less than equal to 10000.

So, instead of that lets think of an alternate way. Since we added 999 earlier let's instead subtract 999 from the result, which would mean to subtract the result from 999.

$$
\begin{array}{r@{}r@{}r@{}r}
 & 9 & 9 & 9 \\[-1pt]
- & 9 & 2 & 2 \\ \hline
 & 0 & 7 & 7 \\ \\
\end{array}
$$
This calculation can be done without borrowing.

Now, that we have preformed subtraction without borrowing using decimal numbers let's try them using binary numbers.

Performing the subtraction without borrowing with binary numbers is actually in some ways even easier, let's see how.

Here is the problem we have been working on again: 

$$ \begin{array}{r@{}r@{}r@{}r}
 & 2 & 5 & 3 \\[-1pt]
- & 1 & 7 & 6 \\ \hline
 & 0 & 7 & 7 \\ \\
\end{array}
$$
Let's first convert the numbers into binary.

$$ \begin{array}{r@{}r@{}r@{}r}
& 1 & 1 & 1 & 1 & 1 & 1 & 0 & 1 \\[-1pt]
-  & 1 & 0 & 1 & 1 & 0 & 0 & 0 & 0 \\ \hline
 & ? & ? & ? & ? & ? & ? & ? & ? \\ \\
\end{array}
$$
Let's go through the steps.

 1. We first subtract the ***Subtrahend*** from a string of 9s when the numbers are in decimal, we do this do avoid any borrowing, so when calculating in binary we will subtract from a string of 1s, appropriately called taking the 1s complement.
 
Looking deeper we realize that calculating the 1s complement of something is very easy and avoids doing any subtraction whatsoever. This is because any 0 in the original number becomes a 1 and any 1 becomes a 0, so we can simply use a **NOT** operator to negate the bits to get our result.

Here is the result of step 1.

$$ \begin{array}{r@{}r@{}r@{}r}
& 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 \\[-1pt]
-  & 1 & 0 & 1 & 1 & 0 & 0 & 0 & 0 \\ \hline
 & 0 & 1 & 0 & 0 & 1 & 1 & 1 & 1 \\ \\
\end{array}
$$

2. Add the 1s complement of the ***Subtrahend*** to the ***Minuend***.

$$ \begin{array}{r@{}r@{}r@{}r}
& 1 & 1 & 1 & 1 & 1 & 1 & 0 & 1 \\[-1pt]
+  & 0 & 1 & 0 & 0 & 1 & 1 & 1 & 1 \\ \hline
1 & 0 & 1 & 0 & 0 & 1 & 1 & 0 & 0 \\ \\
\end{array}
$$
3. Add one to the result.

$$ \begin{array}{r@{}r@{}r@{}r}
1 & 0 & 1 & 0 & 0 & 1 & 1 & 0 & 0 \\[-1pt]
+  &  &  &  &  &  &  &  & 1 \\ \hline
1 & 0 & 1 & 0 & 0 & 1 & 1 & 0 & 1 \\ \\
\end{array}
$$
4. Subtract 11111111  (which equals 256) from the result.

$$ \begin{array}{r@{}r@{}r@{}r} \\
& 1 & 0 & 1 & 0 & 0 & 1 & 1 & 0 & 1 \\[-1pt]
- & 1  & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 \\ \hline
& 0 & 0 & 1 & 0 & 0 & 1 & 1 & 0 & 1 \\ \\
\end{array}
$$

The final difference is 77 in decimal as expected.

Let's try it again with the numbers reversed.

$$ \begin{array}{r@{}r@{}r@{}r} \\
& 1 & 0 & 1 & 1 & 0 & 0 & 0 & 0 \\[-1pt]
-  & 1 & 1 & 1 & 1 & 1 & 1 & 0 & 1 \\ \hline
 & ? & ? & ? & ? & ? & ? & ? & ? \\ \\
\end{array}
$$

1. We subtract the ***Subtrahend*** from 11111111.

$$ 
 \begin{array}{r@{}r@{}r@{}r} \\
& 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 \\[-1pt]
-  & 1 & 1 & 1 & 1 & 1 & 1 & 0 & 1 \\ \hline
 & 0 & 0 & 0 & 0 & 0 & 0 & 1 & 0 \\ \\
\end{array}
$$
2. We add the one's complement of the ***Subtrahend*** to the ***Minuend***.

$$
\begin{array}{r@{}r@{}r@{}r} \\
& 1 & 0 & 1 & 1 & 0 & 0 & 0 & 0 \\[-1pt]
+  & 0 & 0 & 0 & 0 & 0 & 0 & 1 & 0 \\ \hline
 & 1 & 0 & 1 & 1 & 0 & 0 & 1 & 0 \\ \\
\end{array}
$$
Now, since we effectively added 11111111 earlier we need to subtract it as well, usually we would add 1 to the result and subtract 100000000. But that is not possible without borrowing. So, instead we subtract this result from 11111111.

$$
\begin{array}{r@{}r@{}r@{}r} \\
& 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 \\[-1pt]
-   & 1 & 0 & 1 & 1 & 0 & 0 & 1 & 0 \\ \hline
 & 0 & 1 & 0 & 0 & 1 & 1 & 0 & 1 \\ \\
\end{array}
$$

The result is equal to 77 in decimal, actually -77.

Great now that we have the knowledge to perform binary subtraction let's attempt to design a circuit to do exactly that. To keep things simple our circuit will only do subtractions where the ***Subtrahend*** is smaller than the ***Minuend***.

Let's recall the circuit we used for addition: 

![[binary subtraction 1.png]]

This is an 8 bit adder with 8 outputs, the carry out is connected to the 9th output and is used to indicate if the result is greater than 255.

The control panel looked like this.

![[binary subtraction 2.png]]

The new control panel that incorporates subtraction looks like this: 

![[binary subtraction 3.png]]

This control panel includes a new switch which will be 0 for addition and 1 for subtraction. The ninth output has been replaced with a Overflow/Underflow lamp which is 1 for when the result of the addition is greater than 255 and 1 when the difference is negative i.e. when the ***Subtrahend*** is larger than the ***Minuend***.

Now, let's try to design the circuit that also does subtraction.

First of we need to design a circuit that computes the 1s complement of the ***Subtrahend***. For that we can just use an array of **NOT** gates.

![[binary subtraction 4.png]]

The issue with this is that this always inverts the inputs, we only need to invert the inputs when the SUB signal is 1. A better circuit looks like this