---
layout: post
title: Review of Tools and CSS Setup
description: My journey through creating a Github Pages site
type: tangibles
courses: { compsci: {week: 2} }
---

<style>
    body {
        background-color: #1c1c1b;
        color: #fff;
    }

    a {
        color: #3498db;
    }
    code {
        color: green;
    }
</style>

## VSCode
1. I installed [VSCode](https://code.visualstudio.com/) from the website that is linked
2. The installer asked for some additional information and I made sure to choose “Add to PATH”.
   - To my understanding, in Windows, the "PATH" is an environment variable that tells the system where to find executable files. When you add a directory to the PATH, you enable the system to locate and run programs from that directory without specifying the full path. This is especially useful for accessing command-line tools or applications conveniently from any location in the command prompt or PowerShell
3. Add "Remote Development extension pack" in VSCode
   - This allows me to edit in VSCode while also syncing remotely

## WSL
1. I went to Powershell as an admin and ran: <code>wsl --install</code>
2. I restarted my computer
3. I set up my WSL account with a username and password

## Theme
### Once having the site up and running, I didnt really like the original theme. To have more of a say in the look of my website, I used the Minimal theme and added a LOT of CSS. To do this: 
1. I went to https://github.com/pages-themes/minimal
2. Once on the site, I made sure my remote theme was set to <code>remote_theme: pages-themes/minimal@v0.2.0</code>
3. After doing this, I made sure to copy the code from <code> _layouts/default.html </code> into my default.html
4. I did a lot of CSS to make the site how it is now!