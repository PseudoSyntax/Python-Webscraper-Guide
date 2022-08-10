[![Stargazers][stars-shield]][stars-url]
[![Issues][issues-shield]][issues-url]


&nbsp;
&nbsp;

[stars-shield]: https://img.shields.io/github/stars/PseudoSyntax/Python-Webscraper-Guide.svg?style=flat-square
[stars-url]: https://github.com/PseudoSyntax/Ubuntu-20.04-Kernel-Setup-Instructions/stargazers
<!--style=flat-square   and  style=for-the-badge-->
[issues-shield]: https://img.shields.io/github/issues/PseudoSyntax/Python-Webscraper-Guide.svg?style=flat-square
[issues-url]: https://github.com/PseudoSyntax/Python-Webscraper-Guide/issues


<!-- PROJECT LOGO -->

<p align="center">
  <a href="https://sketchywebsite.net/">
     <img width="72" alt="Chicken1" width="80" height="80" src="https://user-images.githubusercontent.com/43308680/132917546-e46cdfeb-0f53-4868-af4c-9c3a0742a332.PNG">
  </a>
  
  
  <h3 align="center">Mr.ChickenCombo's Guide to Python Web Scraping</h3>

  <p align="center">
    An great guide towards making your own personal Python Webscraper!
    <br />
    <a href="https://github.com/PseudoSyntax/Python-Webscraper-Guide/wiki"><strong>Explore the docs »</strong></a>
    <br />
    <br /> 
    <a href="https://github.com/PseudoSyntax/Python-Webscraper-Guide">Home</a>
    ·
    <a href="https://github.com/PseudoSyntax/Python-Webscraper-Guide/issues">Report an Issue</a>
    ·
    <a href="https://github.com/PseudoSyntax/Python-Webscraper-Guide/wiki">Webscraper Wiki</a>
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
    <li><a href="#Choosing-a-Website-to-Scrape">Choosing a Website to Scrape</a></li>
    <li><a href="#Scraping-Website-HTML">Scraping Website HTML </a></li>
    <li><a href="#Scraping-JSON-Values-from-a-Website">Scraping JSON Values from a Website</a></li>
    <li><a href="#Scraping-a-Website-that-Requires-JavaScript-Input">Scraping a Website that Requires JavaScript Input</a></li>
    <li><a href="#Resources">Resources</a></li>
    <li><a href="#Summary">Summary</a></li>
  </ol>
</details>

## Dependencies
Python 3.XX is needed for this tutorial, as well as the following imports

```
import urllib
import json
import requests
import re
```

## Choosing a Website to Scrape

This simple tutorial is reliant on websites that do not require authentication before the POST response to the web page your script is trying to reach.

For this tutorial the three websites that will be used are the [wikipedia](https://en.wikipedia.org/wiki/Python_(programming_language)) python page, [virus total](https://www.virustotal.com/gui/home/upload) and [mxtoolbox](https://mxtoolbox.com/).


## Scraping Website HTML 
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




## Scraping JSON Values from a Website
  
Now that we have our headers all we need is a "target". For our purposes our objective is to scrape the the virus total analysis of the Wanna Cry sha256 hash and to print the results to the terminal. In order to do this we will be creating a response header to scrape the AJAX cell data from virus total.
  
![image](https://user-images.githubusercontent.com/43308680/183725863-6fad4783-6620-4d15-bc6c-ab05156349b5.png)

Response headers at least according to mozilla is an HTTP header that can be used in an HTTP response and that doesn't relate to the content of the message. Response headers, like Age, Location or Server are used to give a more detailed context of the response.

Not all headers appearing in a response are categorized as response headers by the specification. For example, the Content-Type header is a representation header indicating the original type of data in the body of the response message (prior to the encoding in the Content-Encoding representation header being applied). However, "conversationally" all headers are usually referred to as response headers in a response message.

The idea in our script is to provide some sort of basis when visiting the website. Below you can find a pre-made header that will work with most sites when your looking for something in the HTML.
 
```python
My_header = {
    "User-Agent": "Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:91.0) Gecko/20100101 Firefox/91.0",
    "X-Tool": "vt-ui-main",
    "X-VT-Anti-Abuse-Header": "MTA3OTM2NjUwMjctWkc5dWRDQmlaU0JsZG1scy0xNjMxMTE3NzQyLjY1",
    "Accept-Ianguage": "en-US,en;q=0.9,es;q=0.8",
}
```
  
  

In order to find the AJAX cell data for the webpage we must open the inspect element web tools and search for the response file that conatins what we need. After opening inspect element refresh the page or type CTRL-r to capture the network logs. Once inside type CTRL-f and search for the hash we are looking up(in our example 4a468603fdcb7a2eb5770705898cf9ef37aade532a7964642ecd705a74794b79). When searching the results we want to look for a response file with lots of JSON data. Once we know which file we are looking for copy the path indicated in the picture by the blue box. 
  
![image](https://user-images.githubusercontent.com/43308680/183736430-d209c99f-e3a5-4562-97c7-0af43fe9cb65.png)




Create a variable to hold the path value taken from the previous image.
```python
My_URL = "https://www.virustotal.com/ui/files/4a468603fdcb7a2eb5770705898cf9ef37aade532a7964642ecd705a74794b79"
```
  
Next indicate what headers will be used when visiting the site.
```python
response = requests.get(My_URL, headers = My_header)
```
  
We can now set a variable to store our JSON response file text. The function call json.loads() method can be used to parse a valid JSON string and convert it into a Python Dictionary. It is mainly used for deserializing native string, byte, or byte array which consists of JSON data into Python Dictionary.
  
```python
HTML_data = json.loads(response.content)
```

Now that the response file has been copied we should look into what data we want to aquire. Return to the web page and on the network log where we found the JSON data, copy the response and paste it into a seperate text file on your desktop.

![image](https://user-images.githubusercontent.com/43308680/183745286-cde578a3-97c4-4c38-af7e-6d8fbee7d43b.png)

Once we have the JSON data we must indicate to python from what cell we will be calling data from. The way the AJAX cells navigate is by the path of the parent node all the way to the child node you are searching. In this example below.

![image](https://user-images.githubusercontent.com/43308680/183767675-aa6e6e22-da85-42dc-a05d-2358ce47ae06.png)

  
We would want to call starting from the parent node "data" each proceeding node that includes the node of the value we want. If we are trying to aquire the value of "confidence" we would call it in this order. It should print out the value of 1. 
```
variable1 = response_var['data']['attributes']['sandbox_verdicts']['Zenbox']['confidence']
```
  

Using the method just mentioned above we can aquire the virus total saftey indicators like so:
```python
malicious = HTML_data['data']['attributes']['last_analysis_stats']['malicious']
undetected = HTML_data['data']['attributes']['last_analysis_stats']['undetected']
harmless = HTML_data['data']['attributes']['last_analysis_stats']['harmless']
  
print(f'Malicous Domain: {malicious}\n Undetected Domain: {undetected}\n Harmless Domain: {harmless}')
```

From here when you print the data out it should look something like this:
  
![image](https://user-images.githubusercontent.com/43308680/183768219-22ddcd0e-7b3d-4347-93be-31e50d78aa57.png)

The entire code block will look like this:
```python
import requests
import json


My_header = {
    "User-Agent": "Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:91.0) Gecko/20100101 Firefox/91.0",
    "X-Tool": "vt-ui-main",
    "X-VT-Anti-Abuse-Header": "MTA3OTM2NjUwMjctWkc5dWRDQmlaU0JsZG1scy0xNjMxMTE3NzQyLjY1",
    "Accept-Ianguage": "en-US,en;q=0.9,es;q=0.8",
}
My_URL = "https://www.virustotal.com/ui/files/4a468603fdcb7a2eb5770705898cf9ef37aade532a7964642ecd705a74794b79"
response = requests.get(My_URL, headers = My_header)
HTML_data = json.loads(response.content)
  
malicious = HTML_data['data']['attributes']['last_analysis_stats']['malicious']
undetected = HTML_data['data']['attributes']['last_analysis_stats']['undetected']
harmless = HTML_data['data']['attributes']['last_analysis_stats']['harmless']
  
print(f'Malicous Activity: {malicious}\n Undetected Activity: {undetected}\n Harmless Activity: {harmless}')
```

## Scraping a Website that Requires JavaScript Input

Certain websites have dta you want to scrape, but don't appear on the html when you search for it. One such website is [MXToolBox](https://mxtoolbox.com/SuperTool.aspx). If we were to [search for mx records on gmail.com](https://mxtoolbox.com/SuperTool.aspx?action=mx%3agmail.com&run=toolpage) we would not get any of the mx record values in the static html. To work around this we need to view the network response file that loads this data on to the static html page.
  
For our purposes our objective will be to print the first IP address on the MX record list.

![image](https://user-images.githubusercontent.com/43308680/183990918-0cc1f96a-d412-4fa5-95d1-36e33a690d13.png)
















