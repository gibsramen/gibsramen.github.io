---
layout: post
title: Musings on (Neo)Vim
category:
  - Blog
tags:
  - vim
  - neovim
---

I spend *a lot* of time editing text.
Whether it's coding, writing up papers, or procrastination by slapping together blog posts (🤔), my fingers move across the keyboard quite frequently throughout the day.
Since about the end of my undergraduate degree (~mid 2018) I've been using Vim for most text editing.
Why did I start using Vim?
Well, it was mostly because I had a summer research project that required me to SSH into a remote server.
When I looked up the best way to code for a remote machine, I learned about Vim and its ubiquity on \*nix systems.
It was a terrible idea to try to learn Linux & Vim simultaneously while also trying to work on my project but I'm not known for having good ideas.
Through a combination of vimtutor, YouTube videos, and Googling, I slowly but surely started to feel more comfortable with Vim.
I still have a lot to learn about the intricacies of the Vim ecosystem but I definitely consider my editing skills at least past the beginner level.

Throughout the past couple of years I've explored the various plugins for Vim to determine what fits in with my workflow.
One of the plugins that I really like is [ALE](https://github.com/dense-analysis/ale).
ALE is great for linting which is super useful for my coding endeavors.
However, I found that it was difficult to get the code autocompletion like in most IDEs.
I had previously played around with [CoC](https://github.com/neoclide/coc.nvim) but I had trouble getting the autocompletion to work with conda.
I also kept seeing people in the (super cool and not at all nerdy) Vim community talking about [Neovim](https://neovim.io/).
The advantages to Neovim that I saw, namely a more community-driven approach, didn't really matter to me at the time.
Though as I started to work more and more in software, I realized how important such an approach was to software.
A couple days ago, I decided to make the switch to Neovim to see how it was.

It turns out... not much changed. There isn't a super significant difference in how I edit text.
This was a bit anticlimactic given how much I'd heard about Neovim but at the same time that was a huge relief.
Using Neovim now makes me feel better about supporting OSS and I think I'll support the project monetarily given how much utility I get out of it.
At the same time, I decided to take another whack at using CoC.
After a bunch of tweaking and Googling, I think I've finally figured out a workflow that works for me.

I use CoC for both linting and autocompletion now (mainly for Python) and it is super snappy and intuitive.
I also use [vim-slime](https://github.com/jpalardy/vim-slime) to interact with the (i)Python REPL through tmux.
The breadth of configuration options for highlighting, error displays, etc. allow me to personalize everything just to my liking.
If you haven't ever seen my terminal, you'll know that I love coloring everything.
My lizard brain sees multicolored words and finds it much easier to concentrate than just a black and white interface.
Now, I feel comfortable and (sheepish as it may sound) powerful with my editing.
Along with autocompletion, I've configured things such that I can jump to definitions & references, check documentation, and more.
I know a lot of people think that Vim isn't designed to emulate and IDE but I don't really agree.
I think Vim is a blank slate for anyone to use as they please - and for me this means tricking it out with a bunch of bells and whistles.

I've been using Vim for about two and a half years now and I'm excited to keep learning and using this amazing program.

### Plugins I use:

* CoC
* vim-fzf
* vim-fugitive
* vim-surround
* vim-airline
* vim-slime
* vim-tex
* NERDTree
* limelight
