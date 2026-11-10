## Setting: Learning from Bounded Losses

We begin with a simple setting that will be used throughout this example talk.

- A finite policy class $\Pi$, with $|\Pi|=M$.
- Independent samples $Z_1,\ldots,Z_n$ from an unknown distribution $P$.
- A loss function $\ell(\pi,Z)\in[0,1]$.

$$
L(\pi):=\mathbb E_{Z\sim P}[\ell(\pi,Z)],
\qquad
\widehat L_n(\pi):=\frac1n\sum_{i=1}^n\ell(\pi,Z_i).
$$

<div class="block">

**Goal:** Learn a policy with small excess risk.

$$
L(\widehat\pi)-\min_{\pi\in\Pi}L(\pi).
$$

</div>

<!--
Replace this example setting with your own notation.
Use a plain black box for definitions, assumptions, or the problem objective.
-->

---

## Motivation: From Training Loss to Performance

The setup gives us a measurable objective, but evaluation uses fresh data.

- **What we can compute:** empirical loss on the training samples.
- **What we care about:** expected loss on unseen samples.

<div class="spacer"></div>

<v-click>

The key issue is <span class="emphasis">selection using the same data</span>.

$$
\widehat\pi\in\arg\min_{\pi\in\Pi}\widehat L_n(\pi).
$$

A guarantee for one fixed policy is not enough: the selected policy depends on the samples.

</v-click>

<v-click>
<div class="block block-observation">

**Idea:** Control estimation error <span class="emphasis">uniformly over the policy class</span>.

</div>
</v-click>

<!--
Use the arrow keys to reveal the motivation in two steps.
Red is reserved for the mathematical obstacle or the main insight.
PDF export shows the completed slide by default.
-->

---

## Key Observation: Uniform Control Handles Selection

A uniform estimation bound lets us analyze the policy chosen from the data.

<div class="block block-observation">

**Key observation:** If every policy satisfies

$$
|\widehat L_n(\pi)-L(\pi)|\leq\varepsilon,
$$

then an empirical minimizer satisfies

$$
L(\widehat\pi)-\min_{\pi\in\Pi}L(\pi)\leq\textcolor{red}{2\varepsilon}.
$$

</div>

**Why it helps:** The same event controls both the selected policy and the optimal comparator.

<!--
Yellow boxes highlight an observation, contribution, or central idea.
This is a standard illustrative fact, not a claim of novelty.
-->

---

## Main Result: A Finite-Class Guarantee

We now bound the uniform estimation error to obtain an excess-risk guarantee.

<div class="block block-theorem">

**Theorem (illustrative).** For $0<\delta<1$, with probability at least $1-\delta$,

$$
L(\widehat\pi)-\min_{\pi\in\Pi}L(\pi)
\leq 2\sqrt{\frac{\log(2M/\delta)}{2n}}.
$$

</div>

**Proof:** <span class="emphasis">Uniform concentration</span> + empirical optimality.

<div class="block block-theorem">

**Lemma.** Under the same assumptions, with probability at least $1-\delta$,

$$
\max_{\pi\in\Pi}|\widehat L_n(\pi)-L(\pi)|
\leq\sqrt{\frac{\log(2M/\delta)}{2n}}.
$$

</div>

<!--
Blue boxes are used for theorem and lemma statements, matching the reference deck.
The lemma follows from Hoeffding's inequality and a union bound over M policies.
All policies use the same independent sample; losses need not be independent across policies.
-->

---

## Proof: Two Estimation Errors and One Comparison

The uniform event controls the two stochastic terms in the excess-risk decomposition.

Let $\pi^\star\in\arg\min_{\pi\in\Pi}L(\pi)$. On the event in the lemma,

$$
\begin{aligned}
L(\widehat\pi)-L(\pi^\star)
={}&\underbrace{L(\widehat\pi)-\widehat L_n(\widehat\pi)}_{\leq\varepsilon}\\
&+\underbrace{\widehat L_n(\widehat\pi)-\widehat L_n(\pi^\star)}_{\leq0\text{ by empirical optimality}}\\
&+\underbrace{\widehat L_n(\pi^\star)-L(\pi^\star)}_{\leq\varepsilon}
\leq 2\varepsilon.
\end{aligned}
$$

<div class="block">

**Conclusion:** Uniform concentration makes the argument valid for the data-dependent policy.

</div>

<!--
Keep central proofs in the main talk when they are short enough to explain.
The formula is ordinary KaTeX, so it remains editable in Markdown.
-->

---

## Method: Evaluate, Compare, Select

The proof suggests a direct learning procedure for this finite-class example.

<div class="columns">
<div>

1. Collect $n$ independent samples.
2. Evaluate each candidate policy.
3. Select an empirical minimizer.
4. Evaluate on independent test data.

$$
\widehat\pi\in\arg\min_{\pi\in\Pi}\widehat L_n(\pi).
$$

</div>
<figure>
<img src="/figures/method.svg" alt="Independent samples feed an empirical-loss calculation, which selects a policy." />
<figcaption>A simple figure beside the method.</figcaption>
</figure>
</div>

<div class="block block-observation">

**Practical question:** Can we search the policy class <span class="emphasis">efficiently</span>?

</div>

<!--
Replace public/figures/method.svg with your own figure.
Keep the claim and the figure side by side without adding decorative cards.
-->

---

## Summary

<div class="block block-observation">

**Main idea:** A uniform error bound supports data-dependent policy selection.

</div>

- **Method:** Minimize empirical loss over a finite policy class.
- **Analysis:** Concentration + a union bound + empirical optimality.
- **Trade-off:** More candidates increase both search cost and statistical complexity.

<div class="block block-theorem">

**Guarantee:** With high probability, excess risk scales as

$$
\mathcal O\!\left(\sqrt{\frac{\log(2M/\delta)}{n}}\right).
$$

</div>

**Open question:** How should we extend this argument to richer policy classes?

<!--
End with one main idea, one guarantee, and one natural next question.
Replace all illustrative mathematical content when using this template for your own talk.
-->
