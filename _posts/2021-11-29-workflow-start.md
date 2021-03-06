---
layout: post
title: "Creating a Data Science Workflow"
date:   2021-11-29 22:52:18 -0600
categories: [vim, data-science]
published: False
---

# Creating a Data Science Workflow: Starting Out

The aim of this post is to share what my experience of setting up a workflow has been so far. I'm new at a lot of this, and it's still very much a work in progress. As things change, I plan on writing follow ups. Until then advice and suggestions are welcome!

As a new data science student, I've been looking to create a productive environment. Partly this is because any edge I can get on workspeed now will pay off as the grind continues to ramp up, but I also have to admit that I enjoy going down setup rabbit holes. I delight in adding plugins to my editor. This trigger happy approach to modifying tools of course has the potential to cause problems. I will likely have to focus more on streamlining as the course goes on, but right now I can enjoy myself a bit.

### Introduced tools
Flatiron started us out with a conda environment and jupyter. VS Code was also an encouraged option, though not immediately relevant as we've just needed to work with notebooks so far. We'll be using Github for version control and collaboration. All of this appears to be quite standard so far given my admittedly limited knowledge of data science practices at this point. With that being said, I have taken a few python courses before, so I was already using pycharm. On the whole I liked it, but startup times were long, and I was a novice user. This has made the change to VS Code relatively painless as I didn't have really any pycharm expectations that needed to be revised.

### Steps towards more vim integration
Up until now, it may seem as if all my needs were met, if I hadn't been a vim fan. Like with pycharm, I was not an experienced vim user, but going through `vimtutor` was enough to make me at the very least want h,j,k,l keybindings everywhere. Conveniently, VS Code has multiple extensions for vim keybindings. I added both the vim and neovim extensions, since at my current level, it likely won't make a huge difference. I've been using neovim lately, that plugin is more positively reviewed and as it advertises runs a full nvim instance so the behavior should hopefully be accurate. This does leave a problem. Editing `.py` files with this extension is great! But when I want to edit a notebook, I no longer have the ability to activate the hover documentation. This is done with `shift tab` in jupyter lab and `CTRL+K CTRL+I` in normal VS code. It's not the biggest of issues, but if the entire point of changing things up is to add speed, why put myself in a position where i have to go look up documentation in the browser. Currently it hasn't been an issue, but I might start using jupyter lab for notebooks now. This may have to do with VS code context settings, so that's been added to my todo list (literally, more on that later).


### Windowing with regolith
The next big piece of the setup puzzle for me is windowing. Specifically terminal windows. Starting a jupyter instance from the command line means that terminal session is not immediately usable through the same window, you have to either open a new window or use a terminal pane from inside jupyter. This i find unsatisfactory. Some poking around initially pointed me a tmux, a popular terminal window manager. I was able to install and use it relatively painlessly, but there were a few issues. An annoyance was that it ruined my syntax highlighting in vim. Stack overflow had a few ideas for how to fix this, but after some tinkering, the general impression I was left with was that it would be too much of a time sink to get it going. And besides that I had warmed up to the idea of tiling window management. [This](https://www.reddit.com/r/vim/comments/r0mou0/as_a_complete_n00b_id_like_to_share_some_of_my/) reddit post was fairly inspiring, but I didn't want to do that level of change. We have to do a fair ammount of pair programming which a total shakeup makes more difficult. Besides, what was really interesting to me was tiling windows more than messing with system keybinds.

This eventually brought me to Regolith. Regolith is a bundling of software to create an environment rather than an original creation. It's based off the i3 windowing manager which is a popular choice for GNOME systems, but has a pretty steep learning curve and isn't particularly useful until it's configured. Regolith is essentially a preconfigured i3, notably moving the $mod functionality from the `alt` key to the `super` key (windows key or mac command key). In regolith, the `super` key shorcuts do most of the work you might otherwise do with the mouse. `super space` brings up an app launcer where you type the name you want and hit `enter` to execute, `super enter` opens a new terminal, and `super shift q` exits an application. Instead of opening a new window on top of another and having to mouse between the two, tiling managers open up another window in splitscreen with the original. This process can be repeated and the window locations customized and adjusted. Eventually of course you run out of screen space at which point you hit `super <num>` to move to another workspace, where you may happily continue adding windows. This has been helpful with the second monitor. Of importance as well was making sure zoom worked with it, and aside from a little resizing being needed, it does.

### Goals
Overall I wanted to continue the keyboard-over-mouse philosophy of vim into more general usage. Regolith in particular has been helpful with that, however there's a long way to go. There are of course many addons to try out, but of course making the system too specific to me can make collaboration more difficult. I've only scratched the surface on this and I'm really looking forward to taking this further.
So far I've really only covered setup, in part this is because it's what we've mostly spent time on over the first 3 days. As the course progresses, I'll post about how I work more generally. As the course progresses, I'll post about how I work more generally. As the course progresses, and I develop my own preferences, I'll post about how I work more generally.


<script src="https://utteranc.es/client.js"
        repo="UpGoerFive/UpGoerFive.github.io"
        issue-term="pathname"
        theme="github-dark"
        crossorigin="anonymous"
        async>
</script>
