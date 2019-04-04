---
layout: post
title: "Use the Pumping Lemma for Regular Languages"
categories:
  - Tutorials
tags:
  - proof
  - automata and languages
  - computer science

last_modified_at: 2019-04-04
excerpt_separator: <!-- more -->
---

This semester I finished my course about automata and languages. I learned a lot and it was really enjoyable. From this field, there was [a question about the Pumping Lemma on the computer science subreddit](https://old.reddit.com/r/computerscience/comments/atrs4y/pumping_lemma_in_theory_of_computation/). So naturally, if someone ask about a thing I know about, I'll try to explain it as best as I can -- repetition is key for retaining knowledge!

<!-- more -->

# My Explanation

[My original post on Reddit.](https://old.reddit.com/r/computerscience/comments/atrs4y/pumping_lemma_in_theory_of_computation/eh3aw9k/)

To understand the PL, we think about it in two steps. I'll do it for the regular languages. You can do the same on your own for the context free languages. The idea is the same.

1. We create a visual model to understand what it is about.
2. We do a PL proof.

## 1. Creating a visual model in your mind.

Firstly, we have to keep in mind that we want to show that a language is not regular. Let's reason a little bit more about regular languages:

- Regular languages can be accepted by finite automata (FA). That means: if your language is regular, there is an FA that accepts this language.
- To check, whether a word is accepted by an FA, you start in a state, start reading in letters of your word and follow the edges through the FA. If the whole word is read and we end up in a final state, the FA accepts the word.
- But hold on a second! FA can also accept words that have more letters than we have states and edges. How can that be?! The answer: loops.
- Now, given any regular language, we **know** that there is an FA that accepts it (this is a theorem).
- That means: if we have a word, that has more letters than we have states, but is still accepted by our FA, we **have to have** a loop in our FA.
- Think about it: we can repeat this loop as many times as we want and the FA would still accept words that are processed by going through the loop repeatedly. **It has to!**
- This repeating of the loop is referred to as pumping a word up or down.


## 2. Doing a proof.
Now, secondly, you want to proof something using this lemma. Let's start with the PL (really try to understand this line!):

`L ∈ REG → ∃n ∈ ℕ ∀x ∈ L: |x| ≥ n ∃u, v, w: x = u ∘ v ∘ w, |v| ≥ 1, |uv| ≤ n ∀i ∈ ℕ: u ∘ vⁱ ∘ w ∈ L`

I'll break it down. Remember: This is a theorem. If you meet the conditions of the implication (part on the left), you now **know** that the part on the right is true.

1. `L ∈ REG →`: "Given a regular language, the following is true."
2. `∃n ∈ ℕ`: "There is a natural number"
3. `∀x ∈ L: |x| ≥ n`: "For every word `x` that is in the language and longer than this natural number"
    - Remember the argument with the loops from part 1. This just says: we now have a word that has more letters than we have states.
4. `∃u, v, w: x = u ∘ v ∘ w, |v| ≥ 1, |uv| ≤ n`: "You can split up the word `x` into three parts: `u, v, w` where the length of `v` is equal to or bigger than `1` and the length of `u ∘ v` is smaller than our natural number from before"
    - Here we describe the loop in more detail. `v` is the part that we can pump, because there is a loop that processes v (and can thus process arbitrary iterations of v - or skip it altogether. And because `u ∘ v` is smaller than `n`, we didn't need a loop until now. We only really **need** a loop, if our word has more letters than we have states!
5. `∀i ∈ ℕ: u ∘ vⁱ ∘ w ∈ L`: "If all the conditions before have been met, we can now pump `v` up or down and the resulting word is still in the language!"
    - Since it is a loop, pumping doesn't make a difference. If you go the loop a million times, the word is still in the language.

That's it. Again, because it is proved, you **know** it's true if all the conditions are met.

We want to use the lemma to show, that a language **is not** a regular language. Let's have a look at the implication from above. Think about the left part of the implication (`L ∈ REG`) as `A` and the right part (`∃n ∈ ℕ ∀x ∈ L: |x| ≥ n ∃u, v, w: x = u ∘ v ∘ w, |v| ≥ 1, |uv| ≤ n ∀i ∈ ℕ: u ∘ vⁱ ∘ w ∈ L`) as `B`: `A → B`.

We can now do the following transformation:

`A → B ≡ ¬B  → ¬A`

To pull in the negation on the right side of this transformation, all the quantifiers have to "flip around". This means the sentence now looks like this:

`∀n ∈ ℕ ∃x ∈ L: |x| ≥ n ∀u, v, w: x = u ∘ v ∘ w, |v| ≥ 1, |uv| ≤ n ∃i ∈ ℕ: u ∘ vⁱ ∘ w ∉ L → L ∉ REG`

Again, this is still the Pumping Lemma. We didn't change it, we just used an transformation for the implication that is equivalent. If you meet the conditions on the left, you **know** the sentence on the right is true.

Let's use this on an example: Show that `L = {aᵏ ∘ bᵏ | k ≥ 0}` is not regular.

1. Take any number `n`.
2. Select a word with the requirement `x ∈ L` with `|x| ≥ n`. Your mathematical creativity is requested here! You need to pick a word that helps you show the rest of the conditions easily! We are going to pick: `x = aⁿbⁿ`. This is convenient, because it’s obvious that it is as least as long as n (n occurs twice in it as an exponent). The important property to note: there are exactly as many `a` as there are `b` in this word. So if we can pump it in a way, that this is not the case anymore, we are golden!
3. Now we have to look at **all** of the partitions `x = uvw` with the conditions `|v| ≥ 1` and `|uv| ≤ n`. Since we have to look at all of them we just say: Let's assume these conditions are met (we can now use them in the next step).
4. Pick an `i` that shows that `u ∘ vⁱ ∘ w ∉ L`. Let's take `i = 0`.
    - Since our word is `aⁿbⁿ` and one of the conditions is `|uv| ≤ n`, we know that `uv` can **only** consist of the letter `a`.
    - And because we have the condition `|v| ≥ 1`, we also know that `v` has to contain **at least** one letter `a`.
    - If we now remove this letter (or maybe its more than one letter, it doesn't matter), the amount of letters `a` in the word `x` is now not equal to the amount of letters `b` in the word.
    - Hence: Our word is not part of the language any more (`u ∘ v⁰ ∘ w ∉ L`), violating the PL.

This shows that `L = {aᵏ ∘ bᵏ | k ≥ 0}` is not a regular language.