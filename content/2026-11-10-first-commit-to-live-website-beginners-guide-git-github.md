Title: From First Commit to Live Website: A Beginner's Guide to Git and GitHub
Date: 2026-11-09
Category: Beginner Guide
Tags: git, github, version-control, github-pages, beginners
Slug: first-commit-to-live-website-beginners-guide-git-github

## The Moment It Clicked

I once overwrote a whole day of work with a single bad save, and no undo button could bring it back. That was the day I understood why developers rely on Git. It is a safety net that remembers every version of your work.

Most beginners meet Git as a few scary commands. But once you see how the pieces fit together, it becomes one of the most useful tools you will ever learn. This guide walks through the path step by step, from your first commit to a website that anyone can visit.

## What Are Git and GitHub, Really?

**Git** is a version control system. It tracks changes to your files over time, so you can go back to any earlier point, compare versions, and work on new ideas without breaking what already works.

**GitHub** is a website that stores your Git projects online. It lets you back up your work, share it, collaborate with others, and even host a websites for free.

A few terms you'll see everywhere:

- **Repository (repo)** : a project folder that Git is tracking.
- **Commit** : a saved snapshot of your changes, with a short message.
- **Branch** : a separate line of work, so you can experiment safely.
- **Remote** : the online copy of your repo, usually on GitHub.
- **Push / Pull** : sending your commits to GitHub, or bringing new ones down.

## Step 1: Set Up Git and Create Your First Repo

Install Git, then tell it who you are:

```bash
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
```

Now create a project and start tracking it:

```bash
mkdir my-first-repo
cd my-first-repo
git init
```

That's it. The folder is now a repository.

## Step 2: Learn the Core Loop

Almost all daily Git work is the same three-step loop:

1. **Edit** your files.
2. **Stage** the changes you want to save: `git add .`
3. **Commit** them with a clear message: `git commit -m "Add intro section"`

Use `git status` often. It tells you what changed, what is staged, and what to do next. A few habits that help:

- **Commit small and often**, so each snapshot has one clear purpose.
- **Write meaningful messages**, such as "Fix typo in header", not "stuff".
- **Check before you commit** with `git status` and `git diff`.

## Step 3: Connect to GitHub

Create an empty repository on GitHub, then link it to your local project:

```bash
git remote add origin https://github.com/your-username/my-first-repo.git
git branch -M main
git push -u origin main
```

After this first push, sending new work is simply:

```bash
git add .
git commit -m "Describe your change"
git push
```

A common beginner mistake is running `git push` without committing first. Push only sends commits, not unsaved or unstaged changes.

## Step 4: Use Branches Safely

A branch lets you try something new without touching your working version.

1. Create and switch to a branch: `git switch -c new-feature`
2. Make and commit your changes there.
3. Switch back: `git switch main`
4. Merge the work in: `git merge new-feature`

If the experiment fails, delete the branch and nothing is lost. This is what makes Git feel safe instead of scary.

## Step 5: Publish a Website with GitHub Pages

A working repo is nice, but a live site is better. GitHub Pages can host a static website straight from your repository, for free.

Here is what a simple publishing setup looks like:

- **Content** : write your pages or blog posts in Markdown.
- **Generator** : use a static site tool such as Pelican, Jekyll, or Hugo to build the HTML.
- **Hosting** : push the result to GitHub, and Pages serves it at `your-username.github.io`.
- **Routine** : build, preview locally, commit, push, and your changes go live.

Always preview locally before pushing, so mistakes never reach your visitors.

## Common Mistakes to Avoid

- Forgetting to **commit** before pushing.
- Committing **secrets** such as passwords or API keys. Use a `.gitignore` file and keep them out of the repo.
- Working directly on `main` for risky changes. Use a branch instead.
- Panicking at errors. Read the message carefully, because Git usually tells you what to try next.

## A Simple Roadmap for Beginners

If you're just starting out, here is a path that works:

1. Install Git and make your first local repo.
2. Practice the edit, add, commit loop until it feels natural.
3. Push a project to GitHub and explore the web interface.
4. Create a branch, make a change, and merge it.
5. Publish a small site with GitHub Pages and update it regularly.

## Final Thoughts

Git can feel confusing at first because it comes with its own vocabulary, but the daily workflow is only a handful of commands. Commit often, write clear messages, use branches for experiments, and push your work online.

The gap between "files on my laptop" and "a project the world can see" is smaller than it looks. The best way to cross it is to make your first commit today.