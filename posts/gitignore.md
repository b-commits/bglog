---
title: 'How I removed a directory from remote repository after adding it to .gitignore'
date: '2022-04-06'
---

**I'd expect the future me to be grateful for this.**

Okay, so I pushed some local changes to the remote branch:

`git add .`
`git commit -m "feat: pushed one too many dirs".`
`git push -u origin master`

But the thing is, I forgot to add the directory to the .gitignore file beforehand.

So now I've got that one `tmp` folder hanging about in the remote repository.

To solve it, you first have to unstage the files from the repository that you wish to be hidden, create a commit and push that to GitHub:

`git rm -r --cached src/products/features/tmp`
`git commit -m "chore: remove that one unwanted dir.`
`git push -u origin master`

It's done, you'll thank me later, future me.
