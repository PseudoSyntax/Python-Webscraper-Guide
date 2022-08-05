[![Stargazers][stars-shield]][stars-url]
[![Issues][issues-shield]][issues-url]
![](https://komarev.com/ghpvc/?username=PseudoSyntax&label=Views&style=flat-square&color=blueviolet)

&nbsp;
&nbsp;

[stars-shield]: https://img.shields.io/github/stars/PseudoSyntax/Ubuntu-20.04-Kernel-Setup-Instructions.svg?style=flat-square
[stars-url]: https://github.com/PseudoSyntax/Ubuntu-20.04-Kernel-Setup-Instructions/stargazers
<!--style=flat-square   and  style=for-the-badge-->
[issues-shield]: https://img.shields.io/github/issues/PseudoSyntax/Ubuntu-20.04-Kernel-Setup-Instructions.svg?style=flat-square
[issues-url]: https://github.com/PseudoSyntax/Ubuntu-20.04-Kernel-Setup-Instructions/issues


<!-- PROJECT LOGO -->

<p align="center">
  <a href="https://sketchywebsite.net/">
     <img width="72" alt="Chicken1" width="80" height="80" src="https://user-images.githubusercontent.com/43308680/132917546-e46cdfeb-0f53-4868-af4c-9c3a0742a332.PNG">
  </a>
  
  
  <h3 align="center">Mr.ChickenCombo's Guide to Kernel Setup</h3>

  <p align="center">
    An great guide towards making your own personal Kernel on Linux!
    <br />
    <a href="https://github.com/PseudoSyntax/Ubuntu-20.04-Kernel-Setup-Instructions/wiki"><strong>Explore the docs »</strong></a>
    <br />
    <br />
    <a href="https://github.com/PseudoSyntax/Ubuntu-20.04-Kernel-Setup-Instructions">Home</a>
    ·
    <a href="https://github.com/PseudoSyntax/Ubuntu-20.04-Kernel-Setup-Instructions/issues">Report an Issue</a>
    ·
    <a href="https://github.com/PseudoSyntax/Ubuntu-20.04-Kernel-Setup-Instructions/wiki">Ubuntu Error Wiki</a>
  </p>
</p>

# Python-Webscraper-Guide

This tutorial will teach the basics of regex webscraping as well as calling JSON response file values via AJAX. 
*(Note: This guide is not fully complete. If you see anything that needs to be emphaisized or changed add a comment )*



<!-- TABLE OF CONTENTS -->
<details open="open">
  <summary>Table of Contents</summary>
  <ol>
    <li>
      <a href="#Dependencies-Needed">Dependencies Needed</a>
    </li>
    <li><a href="#After-you-Install-Ubuntu">After you Install Ubuntu</a></li>
    <li><a href="#Recommend-Optional-Steps-to-Fix-Screen">Recommend Optional Steps to Fix Screen</a></li>
    <li><a href="#Terminal-Stage">Terminal Stage</a></li>
    <li><a href="#Making-a-Syscall-Successfully">Making a Syscall Successfully</a></li>
    <li><a href="#Summary">Summary</a></li>
  </ol>
</details>

## Dependencies Needed
Python 3.XX is needed for this tutorial, as well as the following imports

```
import urllib
import json
import requests
import time
import re
```

## Choosing a Website to Scrape

This simple tutorial is reliant on websites that do not require authentication before the POST response to the web page your script is trying to reach.

For this tutorial the three websites that will be used are the [wikipedia](https://en.wikipedia.org/wiki/Python_(programming_language)) python page, [virus total](https://www.virustotal.com/gui/home/upload) and [mxtoolbox](https://mxtoolbox.com/).

































