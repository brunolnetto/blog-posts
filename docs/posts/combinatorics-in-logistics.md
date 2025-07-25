---
layout: post
title:  "Combinatorics in Logistics: How Hard Can It Be?"
date:   2025-07-25 10:00:00 -0300
---

**How Hard Can Combinatorics in a Logistics NP‑Hard Problem Be?**

*From pure partitions to intractable vehicle‑routing*

---

## Introduction

When you hear “vehicle routing,” you probably think of distance, deadlines, or driver schedules. Yet before any of that, there’s a deceptively simple question lurking beneath:

> **How many ways can you assign and order $n$ delivery points across $m$ vehicles?**

You might expect “lots,” but the true scale is staggering. In this post, we’ll build from elementary counting formulas to the moment you “harden” the problem with real‑world constraints and land squarely in NP‑hard territory.

---

## What We’ll Cover

1. [Setup: Definitions and notation](#1-setup-definitions-and-notation)
2. [Explanation steps](#2-explanation-steps)
4. [Key Takeaways & Conclusion](#3-key-takeaways--conclusion)

---

## 1. Setup: Definitions and notation

* **Delivery points**: $n$ distinct locations
* **Vehicles**: $m$ distinct routes (each serves at least one point)
* **Order matters**: the sequence of stops on each route is significant
* **Goal**: *just count* the number of ways to assign and order all $n$ points among the $m$ vehicles

**Notation**

* $n!$ = number of permutations of $n$ points
* $n-1$ = gaps between consecutive points where we “cut”
* Vehicles labeled $1,2,\dots,m$

---

## 2. Explanation steps

### Step 1: Fixed‑$m$ Count

1. **Permute** all $n$ points ⇒ $n!$ possible sequences.
2. **Cut** that sequence into $m$ non‑empty segments by choosing $m-1$ of the $n-1$ gaps ⇒
   $\displaystyle \binom{n-1}{m-1}$.

   > *Intuition:* cuts occur only *between* points, so no segment can be empty.
3. **Label** each segment by vehicle index $1,2,\dots,m$.

**Total count for fixed $m$:**

$$\text{cardinality for fixed } m = n! \binom{n-1}{m-1}$$

---

###  Step 2: Summing Over All $m$

Allow any number of non‑empty routes $1 \le m \le n$:

$$\sum_{m=1}^n n! \binom{n-1}{m-1} = n! \sum_{k=0}^{n-1}\binom{n-1}{k} = n! 2^{n-1}.$$

Even without constraints, the space grows as **factorial $\times$ exponential**—astronomical for modest $n$.

---

### Step 3: Stirling & Bell Numbers

Before adding **order** inside segments and **labels** on segments, the classic grouping problem is:

* **Stirling numbers of the second kind**, $S(n,m)$, count ways to partition $n$ points into $m$ *unordered*, unlabeled blocks.
* **Bell numbers**, $B(n)=\sum_{m=0}^n S(n,m)$, count *all* unordered partitions of $n$ elements.

> **Example** ($n=3,\ m=2$):
> $\{A,B,C\}$ can split into two unlabeled groups in
>
> $$
> S(3,2)=3
> $$
>
> ways:
> $((A,B),C), ((A,C), B), ((B,C), A).$

Our **ordered, labeled** count,

$$n!\;\binom{n-1}{m-1},$$

is essentially the Stirling count “blown up” by adding two layers of ordering—first ordering the points, then ordering the groups among vehicles.

---

###  Step 4: Generating‑Function Lens

To gain an *analytic* handle on this family of counts, define the bivariate **exponential generating function** (EGF):

$$ G(x,y) = \sum_{n\ge m\ge1} \left[ n! \binom{n-1}{m-1}\right] \frac{x^n}{n!} y^m. $$

* **$x$** tracks the number of delivery points $n$.
* **$y$** tracks the number of vehicles/routes $m$.

By construction, one shows (via the marked‑block construction in analytic combinatorics) that

$$G(x,y) = \frac{y e^x}{\bigl(1 - y e^x\bigr)^2}.$$

From $G(x,y)$ you can extract:

* **Coefficients** $\displaystyle [x^n y^m]$ to recover $n! \binom{n-1}{m-1}$.
* **Moments** of $m$ (expected value, variance) for fixed $n$.
* **Asymptotic growth** as $n\to\infty$.

---

### Step 5: Algorithmic Takeaways

Why does all this matter in practice?

* **Exact enumeration** scales on the order of $\displaystyle n! 2^{n-1}$, so only very small instances (roughly $n\lesssim10$) can be fully listed.
* **Uniform random sampling** from the configuration space is feasible in $O(n)$:

  1. Shuffle the list of $n$ points.
  2. Pick $m-1$ cut‑points uniformly among the $n-1$ gaps.

* **Asymptotic estimates** and average‑case behaviors follow from Stirling’s approximation
  $n!\approx\sqrt{2\pi n} (n/e)^n$ and standard binomial‐tail bounds.

Even this “pure” model explodes so rapidly that exploring the space demands sampling or approximation—brute force is out of the question.

---

### Step 6: Hardening with Constraints

Introduce any realistic constraint, and the neat closed forms evaporate:

1. **Capacity limits** per vehicle
   $\sum\limits_{i\in B_j}d_i \le C_j$ ⇒ combinatorial feasibility checks.
2. **Time windows** ⇒ the Vehicle Routing Problem with Time Windows (**VRPTW**).
3. **Multiple depots** or **heterogeneous fleets** ⇒ the Multi‑Depot Vehicle Routing Problem (**MDVRP**).
4. **Optimization objectives** (minimize distance, fleet size, balance) ⇒ the classic Vehicle Routing Problem (**VRP**).

Once you add these layers, the problem becomes **NP‑hard**—closed‑form counts give way to integer programming, heuristics, or metaheuristics.

---

## 3. Key Takeaways & Conclusion

* **Pure combinatorics** yields elegant formulas ($n!$, binomials, EGFs) but already scales as
  $\displaystyle n! 2^{n-1}$.
* **Stirling/Bell vs. ordered, labeled**: two layers of ordering magnify the space by factorial and binomial factors.
* **Generating functions** provide powerful analytic insights but can’t tame factorial blow‑up.
* **Real‑world constraints** (capacity, time windows, depots, objectives) instantly destroy closed‑form tractability, forcing NP‑hard optimization approaches.

Understanding the **scale** of the unconstrained counting problem is your reality check: even before you optimize routes, you’re facing a combinatorial mountain. Any further “hardening” pushes you into optimization regimes where brute force is impossible—and clever algorithms are your only ally.
