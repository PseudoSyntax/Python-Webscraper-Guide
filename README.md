[![Stargazers][stars-shield]][stars-url]
[![Issues][issues-shield]][issues-url]


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


## Scraping a website that Does Not Require JavaScript Function Call
The website we will be webscraping from will be the Python Wikipedia page. For our purposes our objective is to scrape the latest version of Python as according to the Python Wikipedia page. 

![image](https://user-images.githubusercontent.com/43308680/183219357-484bc5e9-ab83-4fd3-b7ca-519b161364f0.png)

After importing the nessesary Python libraries we will start off by defining a variable that will store our html response with the webpage URL as our argument for the .get() call.
```python
url = requests.get("https://en.wikipedia.org/wiki/Python_(programming_language)")
```
Next decode the response into a string then print the response.
```python
htmltext = str(url.text)
print(htmltext)
```
The terminal should print the HTML of the Wikipedia page like below.

![image](https://user-images.githubusercontent.com/43308680/183713561-0e295cfa-3e65-425d-8951-caed949b6acd.png)

Now in order to obtain just the value of the python version we need to somehow retrive it from the HTML text dump. To do this we will be using ReGex to parse through the data. To formulate a regex for python it is higly reccomended to use [Pythex](https://pythex.org/). For our regex we will be searching for the strings around the value that we would like to aquire. Understand that there are many ways to use and write a regex to fufuil our purposes.

If we take a look at the strings around our value we can identify a unqiue sequence that can help us aquire the value we desire
![image](https://user-images.githubusercontent.com/43308680/183718308-0dc201a1-8a16-432e-b3ab-c0d578d022d5.png)
Using the strings around the value, we can tell Python using the regex (.+) to say to match a group with any characters of 1 or more instances between the strings *style="margin:0px;">* and *<sup id="c* .
```python
Py_Version_Val = re.findall('style="margin:0px;">(.+?)<sup id="c', htmltext)
```
Recall that using re will instantly send your values into a list so to print the value call index 0 since there should only be one instance of what we want on the webpage.
```python
print(Py_Version_Val[0])
```
 The code when put together should look like the lines below and should print the desired output 3.10.6
```python
import urllib
import requests
import re

url = requests.get("https://en.wikipedia.org/wiki/Python_(programming_language)")
htmltext = str(url.text)
Py_Version_Val = re.findall('style="margin:0px;">(.+)<sup id="c', htmltext)
print(Py_Version_Val[0])
```




## Scraping a website with headers

Response headers at least according to mozilla is an HTTP header that can be used in an HTTP response and that doesn't relate to the content of the message. Response headers, like Age, Location or Server are used to give a more detailed context of the response.

Not all headers appearing in a response are categorized as response headers by the specification. For example, the Content-Type header is a representation header indicating the original type of data in the body of the response message (prior to the encoding in the Content-Encoding representation header being applied). However, "conversationally" all headers are usually referred to as response headers in a response message.

The idea in our script is to provide some sort of basis when visiting the website. Below you can find a pre-made header that will work with most sites when your looking for something in the HTML.
 
```python
Wikipedia_headers = {
    "User-Agent": "Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:91.0) Gecko/20100101 Firefox/91.0",
    "X-Tool": "vt-ui-main",
    "X-VT-Anti-Abuse-Header": "MTA3OTM2NjUwMjctWkc5dWRDQmlaU0JsZG1scy0xNjMxMTE3NzQyLjY1",
    "Accept-Ianguage": "en-US,en;q=0.9,es;q=0.8",
}
```

Now that we have our headers all we need is a "target". For our purposes our objective is to scrape the latest version of Python as according to the Python Wikipedia page. 

![image](https://user-images.githubusercontent.com/43308680/183219357-484bc5e9-ab83-4fd3-b7ca-519b161364f0.png)


What we want to do first is specify the URL we are visiting first like this.

```python
My_URL = "https://en.wikipedia.org/wiki/Python_(programming_language)"
```

Next indicate what headers will be used when visiting the site.
```python
My_response = requests.get(My_URL, headers = Wikipedia_headers)
```
Now save the HTML the script will be capturing to a variable.
```python
HTML_data = json.loads(My_response.content)
```
From here when you print the data out it should look something like this:




import requests

url = requests.get("https://en.wikipedia.org/wiki/Python_(programming_language)")
htmltext = url.text
print(str(htmltext))
hy = re.findall('start (.+?)updated',str(htmltext))
print(hy[0])


















