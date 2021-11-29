---
title: "How to Scrape a site with Python"
date: 2021-11-04T11:05:09+03:00
slug: how-to-scrape-with-python
category: ["python"]
summary: А brief guide to scrape a site using Python. You can use this post to get started with Python and develop it further for your personal use. Yes, we will use the programming language and not the green, cute and cold killer that you see in the thumbnail.
description: Knowing the prices of your competitors gives you a big edge when you are a marketer or a small business. Or you had always wanted to know how to get a list of data from a big website without having to go through each webpage and clicking numerous links multiple times. Dig in and find out how you can instruct your computer to do that for you.
cover:
  image: "/img/how-to-scrape/green-python.jpg"
  alt: "A green vicious-looking Python"
  caption: "Focus on scraping like this cute Python"
  relative: "false"
showtoc: true
draft: false
---

### Why you should learn to scrape

This guide will help you in getting started with scraping in no time. Reasons why you would want to scrape in the first place:

* Automate the collection of data from a website.
* Chill while you gloat to yourself that you made a Python script.
* Use the data that is automatically collected to run analysis over it and make conclusions.
* Compare characteristics of a product (especially price) from different websites without manually clicking each web page on your own.

### Things that you will need

1. Python installed on your machine (*By "machine", coders mean the device that you are working on; technically Python can be made to work on mobiles too but stick to a computer or laptop for scraping*) <a href= "https://www.python.org/downloads/"  style = "color : Teal;" target = "_blank"><strong>Follow this link for downloading Python</strong></a>.

2. Install additional modules for Python. Modules are extra add-on libraries to your Python program that save you the trouble of writing your code. *That is the simplest definition of modules.*  You will need **bs4,** **requests** and the **CSV** modules for this scraping task. Follow these steps to get them:  

    1. Open Command Prompt. (If you are not able to find it, then open your Window search bar and type in `cmd`) 
    2. Copy and paste these scripts inside the box on the terminal:   
    `python -m pip install bs4`  
    `python -m pip install requests`  
    `python -m pip install csv` 
    
    These scripts will install the required modules that I mentioned earlier. Their uses will become quite clear when we go through the code to scrape an example site.

3. You will need a code editor to start editing your code somewhere. Code editors are applications like MS Word which let you work with code by providing you different tools that you can use to make your work easy. You can code in notepad if you want, but it will not make much difference. Before saving your code in a file, always remember to give it the proper extension, which in this case is `.py`. So, for example, if you make a scraping script with the name `scraper`, its proper name should be `scraper.py` otherwise your script will not run, since it will not be recognized as a Python script by the Python interpreter.  
If this is your first coding experience, I would suggest you use <a href = https://www.sublimetext.com/ style= "color : LightSalmon;" target = "_blank"><strong>Sublime Text</strong></a> or <a href = https://code.visualstudio.com/download style= "color : SteelBlue" target = "_blank"><strong>VS Code</strong></a>.

That is about all that you will need to make your scraping script!

### Our code, comrade!

I will be using an example site, <a href = http://quotes.toscrape.com/ style = "color: DodgerBlue" target = "_blank" ><strong>Quotes to Scrape</strong></a> to use for our scraping purposes. **Our task, extract all the quotes in a csv file in one column, along with the author's name in the adjacent column.** Hence, one row will have the quote in one column and the author's name in the next column. The structure will be like this:

| **Quote**  | **Author** |
|------------|------------|
|“The world as we have created it is a process of our thinking. It cannot be changed without changing our thinking.”|Albert Einstein|

I will use the scraping code that I wrote, which you can find from my <a href= https://github.com/Sagar-Kap/Quotes/blob/master/Quotes%40.py style = "color : SlateBlue;" target = "_blank" ><strong>GitHub repository</strong></a>

So let's get started:

#### Step 1 : Locating the HTML elements

Open the site and add right-click on the web page and click on **inspect**. Alternatively, you could just hit the `F12` on your keyboard to inspect the HTML code.  
*Do not be afraid* of the console that pops up,it will look something like this:

![Site's HTML inspection](/img/how-to-scrape/site-inspection.png)

Look at the `elements` on the console if it is not already showing its contents by default. Now, look at the site's structure through the HTML tags from the console that opened up. It is called a **Developer Console**. What we require is the quotes from this page, and their author. We will take a look at the HTML document now in front of us. 

Right-click on any of the quote and select `inspect`. You will notice that the quote has been highlighted, and you have been directed to the specific HTML tag on the Developer's Console. Click on expand button of the tag that you are concerned with. What this will do is expand the HTML tag, and you will be able to observe the code better, which in this case happens to be the following tag:

`<div class="quote" itemscope="" itemtype="http://schema.org/CreativeWork">`

This is a `div` tag, with a class named as `quote`. We will pass these values to the Python script, which will then use them to give us the required results. For now, remember that these tags hold the value that you are looking for.

#### Step 2: Controlling your browser through Python!

You read that right, this program that you will write will send a request to this website using your internet and you will not have to do a single thing on your own! Python will take care of it for you. To summarise, the program will do the following:

1. Send a request to `get` the URL that you will pass to the program using the `requests` library.
2. Turn the web page into a soup using the `BeautifulSoup 4` library.
3. Search for the HTML tags that have a quote as a text nested inside them usually they have some <a href = https://www.w3schools.com/html/html_attributes.asp style = "color : MediumSeaGreen;" target = "_blank"><strong>attribute</strong></a> which is the same for all the HTML tags pertaining to the list of things that you might be interested in for scraping, which in this case are the quotes and the authors.
4. Create an array that stores these containers as `list` elements.
5. Run a for-loop in this list and extract the required information from each container and store them in a CSV file. You can print the results on the console as well to check if you are going wrong anywhere. 

To start with your program, create a new file in a folder (also called directories or in short `dir`). Write down the following code:

```py
import requests 
from bs4 import BeautifulSoup as soup
import csv
```
This script will direct the Python interpreter to call the libraries that you installed. You will notice that the BeautifulSoup has been called from bs4, since we require only this specific feature and refer it as `soup`. This makes it easier for us to call it just by typing `soup`, this is done only for the sake of convenience and is not a compulsion. The `csv` module works with CSV files, to store values in comma-separated value files, like MS Excel sheets.

Next, declare a couple of variables which are `URL`, and `page_num`. Their use will become clear later. 

```py
page_num=1
URL = "https://quotes.toscrape.com/page/"+str(page_num)+"/"
```

```py
def get_url(url):
	"""This function performs a GET request to URL
	passed as a parameter within its execution"""                   			 
	with requests.Session() as session:
		return session.get(url)

def make_soup(url):
	"""Function returns a soup object stored in the variable""" 
	return soup(get_url(url).text, "html.parser")
```
On the above code snippet, `get_url()` and `make_soup()` are functions. Functions are blocks of code that you can call later on to do a specific task. Their syntax is like this: 
```py
def <function name>(<parameters>):
    #Block of code here
```
The `get_url()` function will make a connection to the value passed to the URL variable which in this case is <a href = http://quotes.toscrape.com/ style = "color : Tomato;">`http://quotes.toscrape.com/`</a>. If you navigate to the last page of this site by clicking the **next** button, you will find that the total page number is 10. 

> #### So, how does the browser move on to the next page?  
> It is simple. The next page number is added to the earlier url, which increases by a value of 1 from thereon! Just examine the url carefully when you navigate to the next page. You will notice that the value of the original url changes like this: <a href = http://quotes.toscrape.com/page/2/ style = "color : Tomato;">`http://quotes.toscrape.com/page/2/`</a>. 
>
> Do note the <a style = "color : Tomato;">`page/2`</a> at the end of the url now. This is how the url changes with each new page as someone navigates through the website. For every website that you scrape, you will have to navigate through the pages to figure out these two things:  
> 
> 1. How many pages of the site do you want to scrape?
> 2. What are the extra elements that get added when you navigate to the next page?

#### Step 3: Getting the quotes and their author

```py
def get_container_array(url):
	"""Function finds out all HTML containers with the quotes 
	and authors' names and returns an array""" 
	text_boxes_array= make_soup(url).findAll("div", {"class" : "quote"})
	return text_boxes_array

def get_quote(a):
	"""Function finds the quote from the 
	selected HTML container passed to it as an argument""" 
	quote= a.find("span", {"class" : "text"})
	return quote.get_text()

def get_author_name(b): 
	"""Function returns the author's name from the HTML container passed to it
	as an argument"""
	author = b.find("small", {"class" : "author"})
	return author.get_text()
```
The `get_container_array()` function will help you in getting an array of all the HTMl elements that contain the quotes and the author. The site's url is passed to it as an argument. The `get_quote()` function extracts the quote from each container, similarly the `get_author_name()` will extract the author from the given container. Do take note that I had to inspect the HTML tags which held this information. You can inspect them from the developer's console as I have explained before.  

#### Step 4: Getting the info stored in a CSV file

```py
def fill_csv(url):
	"""Function compiles the quotes and 
	author's name into a csv file for all web pages, quotes.csv"""
	with open("quotes.csv", "w", newline="") as file:
		thewriter = csv.writer(file)
		thewriter.writerow(["Quote", "Author"])
		serial_num=1

		for container in get_container_array(url):
			print(str(serial_num)+". " + get_quote(container) + get_author_name(container))
			thewriter.writerow([get_quote(container), get_author_name(container)])
			serial_num+=1
```
The function `fill_csv()` will create a `csv` file named **quotes.csv** and create the first row with the columns **Quote** and **Author**. Then it will use the functions `get_quote()` and `get_author_name()` to extract the quote and its author's name and pass it on to the function `fill_csv()` to print it out in a new row in the **quotes.csv** file. The function runs a <a href = https://www.w3schools.com/python/python_for_loops.asp style = "color : MediumSeaGreen;" target = "_blank">for loop</strong></a> on the array that is created only when `fill_csv()` function is executing. 

#### Step 5 : Running the program on mutiple pages:

```py
def multi_page():
	"""Function will loop through each page
	of site and invoke scraping func fill_csv() on each"""
	Page_Num= 1
	page_url= "https://quotes.toscrape.com/page/"+str(Page_Num)+"/" 

	while Page_Num <= 10:
		fill_csv(page_url)
		Page_Num+=1

multi_page()
```

The function `multi-page()` will run the scraper on the whole site, as you can see in a loop of 10 iterations. The last line of code `multi_page()` is the final nail in the coffin! This is when you call the whole program into action.

### What next?

You can play around with the code, see what you don't understand and if you have a problem, drop me a comment or a message on one of my social media accounts. Doing while learning has its advantages (as well as disadvantages)! I hope this little project provides you some incentive to get started with Python straight away and into a journey of deep learning and frustrations and motivations... Ok, you get the drift. 

![Site's HTML inspection](/img/how-to-scrape/good-luck.png)

### The whole code

```py
import requests 
from bs4 import BeautifulSoup as soup
import csv

page_num=1
URL = "https://quotes.toscrape.com/page/"+str(page_num)+"/"             

def get_url(url):
	"""This function performs a GET request to URL
	passed as a parameter within its execution"""                   			 
	with requests.Session() as session:
		return session.get(url)

def make_soup(url):
	"""Function returns a soup object stored in the variable""" 
	return soup(get_url(url).text, "html.parser")
	

def get_container_array(url):
	"""Function finds out all HTML containers with the quotes 
	and authors' names and returns an array""" 
	text_boxes_array= make_soup(url).findAll("div", {"class" : "quote"})
	return text_boxes_array

def get_quote(a):
	"""Function finds the quote from the 
	selected HTML container passed to it as an argument""" 
	quote= a.find("span", {"class" : "text"})
	return quote.get_text()

def get_author_name(b): 
	"""Function returns the author's name from the HTML container passed to it
	as an argument"""
	author = b.find("small", {"class" : "author"})
	return author.get_text()

def fill_csv(url):
	"""Function compiles the quotes and 
	author's name into a csv file for all web pages, quotes.csv"""
	with open("quotes.csv", "w", newline="") as file:
		thewriter = csv.writer(file)
		thewriter.writerow(["Quote", "Author"])
		serial_num=1

		for container in get_container_array(url):
			print(str(serial_num)+". " + get_quote(container) + get_author_name(container))
			thewriter.writerow([get_quote(container), get_author_name(container)])
			serial_num+=1

def multi_page():
	"""Function will loop through each page
	of site and invoke scraping func fill_csv() on each"""
	Page_Num= 1
	page_url= "https://quotes.toscrape.com/page/"+str(Page_Num)+"/" 

	while Page_Num <= 10:
		fill_csv(page_url)
		Page_Num+=1

multi_page()
```




