# Section 2: Entropy Calculation

Entropy measures the randomness or unpredictability of a password.

## Formula

H = -∑ p(x) log2 p(x)

## Example

For a password with 4 characters:

- Possible characters: 26 letters
- Total combinations: 26^4

H = log2(26^4)  
H = 4 * log2(26)  
H ≈ 18.8 bits

## Analysis

- Longer passwords increase entropy.
- Using more character types increases security.
- Simple passwords have low entropy and are easier to crack.
