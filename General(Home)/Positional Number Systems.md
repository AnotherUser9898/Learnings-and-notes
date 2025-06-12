#positional-number-systems #work-in-progress
**History of Number Systems**

1. Early Additive Systems (Pre-2000 BCE)
    - Civilizations such as the Sumerians, Egyptians, and Romans used additive or non-positional notation.
    - Symbols represented fixed values, summed to form numbers (e.g., Roman numerals: XXVII = 10 + 10 + 5 + 1 + 1).
    - Limitations: no place value, no zero, cumbersome arithmetic.

2. Babylonian Positional System (~2000 BCE)
    - First known positional system, base‑60 (sexagesimal) notation.
    - Used cuneiform symbols; lacked a true zero placeholder.
    - Legacy: modern measurement of time (60 s/min, 60 min/h) and angles (360°).

3. Indian Decimal System (c. 300 – 600 CE)
    - Mathematicians developed a base‑10 positional system including zero.
    - Brahmagupta (628 CE) provided the first formal treatment of zero as a number.
    - Fully featured positional notation: digits 0–9, place value determined by powers of 10.

4. Transmission via Arabic Scholars (700 – 1100 CE)
    - Works of Indian mathematicians translated into Arabic.
    - Al‑Khwarizmi introduced systematic algorithmic methods; term "algorithm" derived from his name.

5. European Adoption (1200 CE onward)
    - Fibonacci’s _Liber Abaci_ (1202) introduced Hindu‑Arabic numerals to Europe.
    - Gradual acceptance over Roman numerals; widespread by the 15th century.

6. Modern Formalization (18th – 19th centuries)
    - Abstract algebra and number theory provided rigorous foundations for positional notation.

# Overview of Number System Types

## 1. Non-Positional Systems

- **Additive Systems**
    - Value obtained by summing symbol values, independent of position.
    - Examples: Roman numerals, Egyptian hieroglyphic numerals.

- **Tally or Unary System**
    - Each mark represents one unit (e.g., |||| = 4).
- **Multiplicative Systems**
    - Symbols combined by multiplication rather than addition.
    - Examples: Mayan long count (mix of base‑20 and base‑18).

- **Ciphered Alphabetic Systems**
    - Letters represent numeric values in fixed ranges.
    - Example: Greek alphabetic numerals (α = 1, β = 2, …, ι = 10, …).


## 2. Alternative Positional and Semi‑Positional Systems

- **Mixed-Base Systems**
    - Different base at each digit position.
    - Examples: Time notation (60 s, 60 min, 24 h), Mayan calendar.

- **Balanced Number Systems**
    - Digits include negative values; allows digits in range '−(b−1)' to '+(b−1)'.
    - Example: Balanced ternary with digits {−1, 0, +1}.

- **Redundant Number Systems**
    - Multiple representations of a single value to optimize carry operations.
    - Example: Carry‑save representation in hardware arithmetic.

- **Bijective Numeration**
    - Digits range from 1 to b (no zero digit). Each integer has a unique representation.
    - Example: Bijective base‑10: 1, 2, …, 9, 10, 11… maps uniquely.
- **Residue Number System (RNS)**
    - Represents numbers by their remainders modulo a set of pairwise‑coprime moduli.
    - Used in digital signal processing and error correction.
- **Complex Base Systems**
    - Uses imaginary or complex bases (e.g., base i, base −1 + i).
    - Primarily theoretical explorations.

# Positional Number Systems

A _positional number system_ with base $b$ represents a number $N$ as:
$$
N = d_n b^n + d_{n-1} b^{n-1} + \dots + d_1 b^1 + d_0 b^0
$$

where each digit $d_i$ satisfies:
$$
0≤di≤b−10 \le d_i \le b - 1
$$
## Purpose and Advantages

1. **Unique Representation**: Each integer or finite fractional value has a single canonical form.
2. **Efficient Arithmetic**: Standard carry and borrow operations correspond to multiplication/division by the base.
3. **Extensibility**: Easily generalized to non‑decimal bases (binary, octal, hexadecimal) in computing.
4. **Compactness**: Compared to additive systems, positional notation requires fewer symbols for large values.
## General Explanation

In a positional system, each digit's value is determined by both:

- **The digit itself** (must be in $0 \le d < b$)
- **Its position** (multiplied by $b^i$, where $i$ is its distance from the least significant digit)

This allows reuse of a small set of digits to represent arbitrarily large numbers.

For example, the digit '2' can mean:

- 2 if in the units place ($2 \times 10^0$)
- 20 if in the tens place ($2 \times 10^1$)
- 200 if in the hundreds place ($2 \times 10^2$)

## Base‑10 (Decimal) Example

$$35210=3×102+5×101+2×100=300+50+2=352352_{10} = 3 \times 10^2 + 5 \times 10^1 + 2 \times 10^0 = 300 + 50 + 2 = 352$$

Digits are in the range 0–9.

## Base‑2 (Binary) Example

$$10112=1×23+0×22+1×21+1×20=8+0+2+1=11101011_2 = 1 \times 2^3 + 0 \times 2^2 + 1 \times 2^1 + 1 \times 2^0 = 8 + 0 + 2 + 1 = 11_{10}$$

Digits are in the range 0–1.

## Base‑8 (Octal) Example

$$2378=2×82+3×81+7×80=128+24+7=15910237_8 = 2 \times 8^2 + 3 \times 8^1 + 7 \times 8^0 = 128 + 24 + 7 = 159_{10}$$

Digits are in the range 0–7.

## Base‑16 (Hexadecimal) Example

Hexadecimal uses digits 0–9 and letters A–F (10–15).
$$2F_{16} = 2 \times 16^1 + 15 \times 16^0 = 32 + 15 = 47_{10}$$
## Why Digits Must Be Between 0 and b−1b - 1

In any positional number system of base bb, digits are restricted to the range:
$$0≤di≤b−10 \leq d_i \leq b - 1$$
This rule exists for three essential reasons: **uniqueness**, **efficiency**, and **canonical form**.
### 1. **Avoiding Ambiguity**

If digits were allowed to be equal to or greater than bb, there would be multiple ways to represent the same number, making the system ambiguous.
#### Example (Invalid Use of Digits Outside Range)

Suppose we use base 10 but allow a digit like 13:

$$2 \times 10^1 + 13 \times 10^0 = 20 + 13 = 33$$

But 33 is already represented canonically as:

$$3 \times 10^1 + 3 \times 10^0=33$$

Thus, allowing digits ≥ 10 creates redundant representations for the same number.

### 2. **Carry Mechanism and Normalization**

In a **positional number system**, digits must satisfy:

$$0≤di≤b−10 \leq d_i \leq b - 1$$

But what if this rule is violated? Suppose we have:

$$di≥bd_i \geq b
$$
Then we must **normalize** the representation. The equation above helps explain how.
### **Step-by-step Explanation**

Let’s say:

- You’re writing a number in base b
- You mistakenly have a digit $d_i$ at position i, where $d_i \ge b$
To fix this:

#### Step 1: Split off exactly one base worth from $d_i$

$d_i = (d_i - b) + b$

Multiply both sides by $b^i$:
$d_{i}b^i=(d_{i} - b)b^i+b \cdot b^i$
$d_{i}b^i = (d_i - b) b^i + 1 \cdot b^{i+1}$

Here’s the interpretation:
- $(d_i - b) b^i$: keep the reduced digit in the current position
- $1 \cdot b^{i+1}$: carry the extra part to the next higher place

This normalization step is purely to convert the whole expression into its simplest form.
### 3. **Uniqueness of Representation (Mathematical Proof)**

#### Theorem:

In a base‑b positional system where digits $d_i \in [0, b-1]$, every non-negative integer has a **unique** representation.

#### Proof Sketch:

Assume two distinct representations for the same number N:

$\sum_{i=0}^{n} d_i b^i = \sum_{i=0}^{m} d_i' b^i$

Assume $d_i \in [0, b-1]$ and $d_i' \in [0, b-1]$. If $\vec{d} \neq \vec{d}'$, then the difference $D = N - N' \neq 0$. But since the base b has unique powers and no digit exceeds b−1, this leads to a contradiction.

Hence, digits restricted to $[0,b−1]$ ensure a **canonical and unique** number representation.
### 4. **Computational Simplicity**

Restricting digits to $[0,b−1]$ allows:

- Easy implementation of arithmetic (carry, borrow).
- Predictable digit ranges.
- Compatibility with algorithms and hardware that assume fixed-size digit sets (e.g., binary logic only needs 0 and 1).
## References and Further Reading

1. David E. Smith, _History of Mathematics_, Vol. 1–3.
2. Donald Knuth, _The Art of Computer Programming_, Vol. 2: Seminumerical Algorithms.
3. G. Ifrah, _The Universal History of Numbers_.
4. N. J. Higham, _Handbook of Writing for the Mathematical Sciences_.