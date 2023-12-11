---
layout: post
title:  "Some fun with variance"
---

<script>
MathJax = {
  tex: {
    inlineMath: [['$', '$']],
  },
  svg: {
    scale: 1
  }
};
</script>
<script id="MathJax-script" async
    src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-svg.js">
</script>

During stats class, I had two questions:
- When you convert random variables to different units, you can simply multiply your mean by that constant. However, for variance you have to square the constant before multipying it. Why does it not follow the rules applicable to the mean?

    $\text{That is, given } F = a + bX$, where $X$ is the random variable:

    $$
    \begin{flalign}
        \text{Mean: } a + b\mathbb{E}[X]\\[3pt]
        \text{Standard Deviation: } b\sigma\\[3pt]
        \text{Variance: } b^{2}\sigma^{2}
    \end{flalign}
    $$

- I had already known the two formulas for calculating variance for random variables.
    - For discrete random variables, it was
    \begin{align}
    \sigma^{2} = \sum_{}(X_{i} - \mathbb{E}[X])^{2}P(X_{i}) = \sum_{}(X_{i})^{2}P(X_{i}) - \mathbb{E}[X]^{2}
    \end{align}
    - For continuous ones, you'd replace the summation with an integral.
    - The first one was straightforward to understand, but I wanted to derive the second one myself to see why it was true.

### First question
This one has a relatively simple answer.

If we take a look at the formula for standard deviation:
\begin{equation}
    \sigma = \sqrt{\frac{\sum{}(X_{i} - \mathbb{E}[X])^{2}}{N}}
\end{equation}

If I multiply all terms of the random variable by the constant $b$, I get:

$$
\begin{align}
\sigma &=  
    \sqrt{
        \frac{\sum{}(bX_{i} - b\mathbb{E}[X])^{2}}{N}
    }\\[8pt]
    &= \sqrt{
        \frac{\sum{}(b(X_{i} - \mathbb{E}[X]))^{2}}{N}
    }\\[8pt]
    &= \sqrt{
        \frac{b^{2}\sum{}(X_{i} - \mathbb{E}[X])^{2}}{N}
    }
\end{align}
$$

Finally, this boils down to:

\begin{equation}
b\sigma = \sqrt{\frac{\sum{}(X_{i} - \mathbb{E}[X])^{2}}{N}}
\end{equation}

From this, it is straightforward to see why variance has a $b^2$.

### Second question
\begin{align}
    \sigma^{2} = \sum_{}(X_{i} - \mathbb{E}[X])^{2}P(X_{i}) = \sum_{}(X_{i})^{2}P(X_{i}) - \mathbb{E}[X]^{2}
\end{align}

Before beginning, it is important to note that $\mathbb{E}[X]$ is a **constant**. With that established, here is how you go from the former one to the latter one:

$$
\begin{align}
    \sigma^{2} &= 
        \sum_{}(X_{i} - \mathbb{E}[X])^{2}P(X_{i})\\
    &= \sum{}{(X_{i}^{2} + \mathbb{E}[X]^{2} - 2X_{i}\mathbb{E}[X]) \times P(X_{i})}\\[8pt]
    &= \sum{}{X_{i}^{2}P(X_{i})} + \sum{}{\mathbb{E}[X]^{2}P(X_{i})} - \sum{}{2 \hskip{0.1cm} X_{i}\mathbb{E}[X]P(X_{i})}\\[8pt]
    &= \sum{}{X_{i}^{2}P(X_{i})} + \mathbb{E}[X]^{2}\sum{}{P(X_{i})} - 2\mathbb{E}[X]\sum{}{X_{i}P(X_{i})}
\end{align}
$$

From here on, we need two important pieces to continue:
- The sum of all probabilities sums to $1$
- The definition for the expected value $\mathbb{E}[X]$ is
    \begin{equation}
        \sum{}{X_{i}P(X_{i})}
    \end{equation}

Now, we can simplify the proof:

$$
\begin{align}
    \sigma^{2} &=
        \sum{}{X_{i}^{2}P(X_{i})} + \mathbb{E}[X]^{2}(1) - 2(\mathbb{E}[X] \times \mathbb{E}[X])\\[8pt]
    &= \sum{}{X_{i}^{2}P(X_{i})} + \mathbb{E}[X]^{2} - 2(\mathbb{E}[X])^{2}\\[8pt]
    &= \sum{}{X_{i}^{2}P(X_{i})} - \mathbb{E}[X]^{2}
\end{align}
$$

Unsurprisingly, changing the summation sign to an integral leads to the variance formula for continuous variables.

\begin{equation}
    \sigma^{2} = \int{}{X_{i}^{2}P(X_{i})} - \mathbb{E}[X]^{2}
\end{equation}

### Remarks
In the end, I was quite surprised by the simplicity of the proof. It was interesting to see why certain things work instead of simply memorizing them and moving on.