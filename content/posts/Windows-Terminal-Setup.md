---
title: "Windows Terminal Setup"
date: "03 May 2022"
showDate: true
comments: true
showSocial: true
showMeta: true
categories:
  - blog
tags:
  - terminal
  - devtools 
  - git
  - linux
  - windows
autoThumbnailImage: false 
thumbnailImagePosition: left
thumbnailImage: https://i.imgur.com/jo21nTU.png
metaAlignment: center
---
  
This is more of a guide than a blog. I will be writing step by step how to configure the windows terminal.

{{< image classes="fancybox center clear" src="https://i.imgur.com/4eiiq10.png" thumbnail="https://i.imgur.com/4eiiq10.png" group="group:windows-terminal" thumbnail-width="70%" thumbnail-height="70%" title="Image of my windows terminal setup" >}}

<!--more-->

1) Download Windows Terminal:
   https://docs.microsoft.com/en-us/windows/terminal/install
2) Make Terminal the "Default terminal application"

{{< image classes="fancybox center clear" src="https://i.imgur.com/coVwG5K.png" thumbnail="https://i.imgur.com/coVwG5K.png" group="group:windows-terminal" thumbnail-width="70%" thumbnail-height="70%" >}}

3) Download PowerShell from the windows store:

{{< image classes="fancybox center clear" src="https://i.imgur.com/acChlV3.png" thumbnail="https://i.imgur.com/acChlV3.png" group="group:windows-terminal" thumbnail-width="70%" thumbnail-height="70%" >}}

4) Set the "Default profile" as PowerShell

{{< image classes="fancybox center clear" src="https://i.imgur.com/1uvL116.png" thumbnail="https://i.imgur.com/1uvL116.png" group="group:windows-terminal" thumbnail-width="70%" thumbnail-height="70%" >}}

5) Hide "Windows PowerShell" from the "settings.json" located in terminal.
    1) Open settings.json
    2) Edit the following lines of code


{{< codeblock "settings.json" "json" "" "settings.json" >}} {
    "commandline": "powershell.exe",  
    "guid": "{61c54bbd-c2c6-5271-96e7-009a87ff44bf}",  
    "hidden": true,  
    "name": "Windows PowerShell"  
},
{{< /codeblock >}}



{{< image classes="fancybox center clear" src="https://i.imgur.com/coVwG5K.png" thumbnail="https://i.imgur.com/coVwG5K.png" group="group:windows-terminal" thumbnail-width="70%" thumbnail-height="70%" >}}
{{< image classes="fancybox center clear" src="https://i.imgur.com/coVwG5K.png" thumbnail="https://i.imgur.com/coVwG5K.png" group="group:windows-terminal" thumbnail-width="70%" thumbnail-height="70%" >}}