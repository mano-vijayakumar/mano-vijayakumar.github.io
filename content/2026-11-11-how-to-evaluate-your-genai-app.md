Title: How to Evaluate Your GenAI App: Build a Small Test Set and Catch Regressions
Date: 2026-11-10
Category: Beginner Guide
Tags: GenAI, LLM, evaluation, evals, prompt-engineering, RAG, testing, beginners
Slug: how-to-evaluate-your-genai-app

## The Prompt Tweak That Broke Everything

I once improved a prompt so that answers sounded friendlier. It felt great in the three examples I tried. Two weeks later, a user told me the app had been giving wrong refund policies the whole time. My "improvement" had quietly broken a case I never re-checked.

That's the problem evaluation solves. Without it, every change to a prompt, model, or retrieval setting is a guess. With it, you can change things confidently and see exactly what got better and what got worse.

## What Does "Evaluating" a GenAI App Mean?

Evaluation (or "evals") is simply a repeatable way to check whether your app gives good answers. Traditional software has unit tests: the same input always gives the same output. GenAI is different because outputs vary and "correct" is often fuzzy. So instead of asking "is this exactly right?", you ask "is this good enough, and is it getting better or worse?"

A few terms you'll see everywhere:

- **Test set (eval set)** : a collection of example inputs, ideally with notes on what a good answer looks like.
- **Metric** : a score that summarizes quality, such as pass rate or average rating.
- **Regression** : when a change makes something that used to work stop working.
- **Baseline** : your current score, the number every change is compared against.

## Step 1: Build a Small Test Set

You don't need thousands of examples. **20 to 50 well-chosen ones** is enough to start catching real problems.

Where to find them:

- **Real user questions**, if you have any. These are the best source.
- **Questions you'd ask yourself** while testing the app by hand.
- **Past failures.** Every time the app gets something wrong, add that case to the set. This is how a test set grows into something valuable.

Aim for variety:

- **Typical cases** : the everyday questions most users ask.
- **Edge cases** : very short inputs, very long ones, typos, mixed languages.
- **Tricky cases** : ambiguous questions or ones needing multiple facts.
- **Out-of-scope cases** : things the app should politely decline or say "I don't know" to.
- **Adversarial cases** : attempts to make the app ignore its instructions.

Store each example in a simple file (a spreadsheet or JSON works fine):

```json
{
  "id": "refund-03",
  "input": "Can I return an opened item after 30 days?",
  "expected": "No. Returns are accepted within 30 days only, unopened items preferred.",
  "tags": ["policy", "returns"]
}
```

Note that the `expected` field describes what a good answer must contain, not an exact string. LLMs phrase things differently every time.

## Step 2: Decide What "Good" Means

Before you score anything, write down your criteria. Common ones:

- **Correctness** : are the facts right?
- **Groundedness** (for RAG apps) : does the answer stick to the retrieved sources, or does it invent things?
- **Relevance** : does it actually answer the question?
- **Format** : is the JSON valid? Is it under the length limit?
- **Tone and safety** : is it on-brand, and does it refuse what it should?

Pick two or three that matter most for your app. Trying to measure everything at once makes results hard to read.

## Step 3: Choose How to Score

There are three main approaches, and most apps use a mix.

**1. Code-based checks.** Fast, cheap, and fully reliable for anything objective: Is the output valid JSON? Does it contain a required keyword? Is it under 200 words? Did it cite a source? Use these wherever you can.

**2. LLM-as-a-judge.** For fuzzy qualities like correctness or helpfulness, you can ask another model to grade the answer against your `expected` notes. A simple judge prompt looks like this:

> "You are grading an AI assistant's answer. Question: {input}. Reference notes: {expected}. Answer: {output}. Does the answer agree with the reference notes and avoid adding unsupported claims? Reply with PASS or FAIL and one sentence of reasoning."

Judges are convenient, but they aren't perfect. They can be biased toward longer answers or toward their own style. Spot-check their grades yourself, especially at the start.

**3. Human review.** The gold standard, but slow. Use it to check a sample of results, to calibrate your judge, and to review the tricky cases.

A good habit: use **binary PASS/FAIL scores** rather than 1 to 10 ratings. They're easier to apply consistently, for humans and models alike.

## Step 4: Run It and Record a Baseline

Write a small script that loops through your test set, runs your app on each input, scores the output, and saves the results. The skeleton is just:

1. Load the test cases.
2. Run the app on each input.
3. Score each output with your checks.
4. Print the pass rate and list the failures.

Run it once on your current setup. That pass rate, say 34 out of 40, is your **baseline**. Save the full results too, not just the number, so you can compare individual cases later.

Because outputs vary, consider setting the temperature low while testing, or running each case a few times if results look noisy.

## Step 5: Catch Regressions

Now the payoff. Every time you change something, re-run the eval and compare to the baseline. Things worth testing this way:

- Editing a prompt
- Switching to a different or newer model
- Changing chunk size or retrieval settings in a RAG app
- Adding or removing a tool
- Upgrading a library

Don't just look at the overall score. Look at **which cases flipped**. A change that goes from 34 to 35 passes might have fixed three cases and broken two others, and those two are exactly what you need to see.

Once you trust the process, run the eval automatically (for example, on every code change) so regressions can't slip in unnoticed.

## Step 6: Keep Improving the Test Set

Your eval is a living thing:

- Every production bug becomes a new test case.
- Remove or fix cases that turn out to be badly written.
- Watch for **overfitting**: if you tune your prompt until it passes every test, you may have tuned it to the test set rather than to real users. Keep a few cases aside that you don't tune against.
- Revisit your criteria as your app and users change.

## Common Beginner Mistakes

- **Testing by vibes.** Trying three examples and calling it good is how my refund bug happened.
- **Waiting for a perfect test set.** Ten imperfect cases today beat a perfect set next month.
- **Only testing happy paths.** The failures that hurt are usually in the edge cases.
- **Trusting the judge blindly.** Always sample and verify its grades.
- **Tracking one big average.** Break results down by category so you can see where things are weak.

## A Simple Starter Checklist

1. Collect 20 to 30 test cases, including a few edge and out-of-scope ones.
2. Write what "good" looks like for each.
3. Add code-based checks for anything objective.
4. Add an LLM judge for the fuzzy parts, and spot-check it.
5. Run it, record your baseline, and save the results.
6. Re-run after every change and review the cases that flipped.
7. Add every new failure to the test set.

## Final Thoughts

Evaluation isn't the glamorous part of building with AI, but it's what separates a demo from a product you can trust. A small test set and a simple script will give you more confidence than any amount of manual poking.

Start tiny. Write ten test cases this week, score them, and save the number. The next time you tweak a prompt, you'll know instead of guess.