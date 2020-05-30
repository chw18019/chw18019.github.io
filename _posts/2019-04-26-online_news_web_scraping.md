---
title: "Web Scraping using Online News Dataset"
date: 2019-04-26
tags: [Web Scraping, Scrapy, Python]
header:
  overlay_image: /assets/190426/freestocks.jpeg
  caption: "Photo credit: [**Freestocks**](https://unsplash.com)"
  teaser: /assets/190426/freestocks.jpeg
excerpt: "Using Scrapy to extract data from websites. (Difficulty: ★★★★☆)"
toc: true
toc_label: "Content"
toc_sticky: true
---
_When I worked on this project, I wondered if I could have more different types of data to predict the popularity, such as title and contents. So, I taught myself and found Scrapy, an open-source framework for extracting the data from websites. For the complete jupyter notebook, you can find it on my GitHub repository. [<i class="fab fa-fw fa-github" aria-hidden="true"></i>](https://github.com/chw18019/Web-Scraping-with-Scrapy)_
{: .notice--primary}

## Introduction
Web scraping is not a new technique, and the first **crawler-based web search engine** can be found in 1993. The history of the web scraping data dates back nearly to the time when the Internet was born. Nowadays, Python is one of the popular languages for web scraping. [This article](https://www.scrapehero.com/python-web-scraping-frameworks/) lists popular Python web scraping framworks and libraries. [Scrapy](https://scrapy.org/) is not the only one you can use, but I found it simple and fast to do the job.

## Fundamental of Web Scraping 
When you click on a link, your computer will send a request to the server for the contents of a webpage. This information is usually returned in `HTML` format. Web scraper performs the same process. Then, it can be used to extract and store the wanted data from webpages. 

## Web Scraping and Web Crawling
In [this article](https://info.scrapinghub.com/web-scraping-guide/beginners-guide-to-web-scraping), it gives a nice explanation about web scraping. It also includes the difference between web scraping and web crawler. The definitions are as following.
> A web crawler, sometimes called a “spider”, is an autonomous bot that systematically browses the internet to index and search for content, by following the internal links on web pages. 

>A web scraper, on the other hand, is a tool designed to accurately extract data from a particular web page.

## Setup
First, you have to install Scrapy by typing the following code in your terminal.
```
$ pip install scrapy
```
Then set up a new Scrapy project by running:
```
$ scrapy startproject tutorial(The Project Name you want)
```
This will create a folder with the following contents.
```
tutorial/
    scrapy.cfg            # deploy configuration file

    tutorial/             # project's Python module, you'll import your code from here
        __init__.py

        items.py          # project items definition file

        middlewares.py    # project middlewares file

        pipelines.py      # project pipelines file

        settings.py       # project settings file

        spiders/          # a directory where you'll later put your spiders
            __init__.py
```

### Spider
> Spiders are classes that you define and that Scrapy uses to scrape information from a website.

In your project, save the code in a file named `quotes_spider.py` under the `tutorial/spiders` directory in your project. Here, I read the `CSV` file into `Pandas` dataframe. Then used the 'URL' to extract title, keyword, description, and article body from the webpages. 
```python
import scrapy
import pandas as pd

df = pd.read_csv("/Users/hua/Documents/Uconn/Class/Semester2/3. Predictive Modeling/Project/OnlineNewsPopularity/OnlineNewsPopularity.csv",delim_whitespace=False)

url = df['url']

class QuotesSpider(scrapy.Spider):
    name = "getkeys"
    start_urls = url.values.tolist()

    def parse(self, response):
        for quote in response.css('h1.title'):
            yield {
                'url':response.url,
                'Title': quote.css('h1.title::text').extract_first(),
                'keyword' :response.xpath("//meta[@name='keywords']/@content")[0].extract(),
                'description':response.xpath("//meta[@name='description']/@content")[0].extract(),
                'parsely-metadata' :response.xpath("//meta[@name='parsely-metadata']/@content")[0].extract(),
                'json': response.xpath("//script[@type='application/ld+json']")[0].extract(),
            }
            
```
Then you can use this code to run the spider to initial the spider. 
```
$ scrapy crawl quotes
```

### Data Prep
After getting the data, I then extract the article body in the 'json' variables. I wrote a function to extract and store the article body into JSON format.
``` Python 
import json
import pandas as pd

with open('/Users/hua/Desktop/Jupyter Notebooks/Web Crawler/getkeys/keys.json', 'r') as myfile:
    data = myfile.read()
    obj = json.loads(data)

df = pd.DataFrame(obj, columns =['url','Title','keyword','description','parsely-metadata','json'])
pd.reset_option("display.max_colwidth")

def extract_articleBody(str):
    words = str.split('":')[3]
    words=words.replace(',"url','')  
    words=words.replace("\n", '')
    words=words.replace("\\", '')
    wordsLower = words.lower()
    for ch in wordsLower:
        if ch in "\.,'?!—":
            wordsLower = wordsLower.replace(ch,'')
    return(wordsLower)

df['articleBody']=''
for i in range(len(df)):
    df['articleBody'][i]=extract_articleBody(df['json'][i])

df.to_json(r'/Users/hua/Desktop/Jupyter Notebooks/Web Crawler/getkeys/contents.json')
```

## Results and Conclusion
For further use, I also saved the data in `CSV` format as the below image shows. You can access the data in my [shared google drive folder](https://drive.google.com/drive/folders/1A_IyPU_sLXgsjtXmtSgBxtcN2IFK6eld?usp=sharing). Scrapy can be used to solve a way more complicated problem than this, such as clicking the next page button and then extract the data from webpages, but in this case, I think it solves the problem just fine. With the exact title, keywords, description, and articles, Nature language Process(NLP) can be performed for further analysis. 

![png](/assets/190426/image1.png)

I hope this post helps you understand and learn how web scraping works. Enjoy learning!
