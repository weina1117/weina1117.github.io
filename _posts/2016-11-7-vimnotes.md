---
layout: post
title: My keypresses on daily basis
---

![sample post]({{site.baseurl}}/images/vim-life.png)

#### Today I'm trying to swich from Textmate to MacVim, here I'm creating an reference of the new commands I've learned as follow:

| Keypresses           |Notes                       |
| -------------------- |:--------------------------:|
| v                    | select                     |
| shift + v            | select row(s)              |
| ctrl + v             | select column(s)           |
| :split               | split screen horizontally  |
| :vsplit              | split screen vertically    |
| ctrl + w + j         |move done a screen          |
| ctrl + w + k         |move up a screen            |
| ctrl + w + h         |move left a screen          |
| ctrl + w + l         |move right a screen         |
| ci + "               |change content in ""        |
| :s/old/new           |to substitue new for the first old in line type     |
| :s/old/new           |to substitue new for all 'old's on a line type      |
| :#, #s/old/new/g     |to substitue phrases between tow line #'s type      |
| :%s/old/new/g        |to substitue all occurrences in the file type       |
| :%s/old/new/gc       |to ask for confirmation each time add 'c'           |
| ctrl + G             |displays location in the file and the file status   |
| G                    |moves to the end of the file                        |
| gg                   |moves to the firlst line of the file                |
| number + G           |monves to that line number                          |
| %                    |the cursor is on a (,),[,],{,or} goes to its match  |
| / + a search pharse  |searches forward for the phrase                     |
|                      |(n for next, N for the opposite directions, ctrl + O go back to older positions, ctrl + I to newer position) |
| ? + a search pharse  |searches backward for the phrase                    |
|                      |(n for next, N for the opposite directions, ctrl + O go back to older positions, ctrl + I to newer position) |

**tips**

* visual block edting:
  1. ctrl + v: active visual block mode
  2. shift + i: to start inserting before every line of the block
  3. esc: apply the insert to every lines