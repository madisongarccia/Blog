---
layout: post
title:  Data Curation Blog Post
date: 2024-03-20
description: 
image: "/assets/img/LA_blog.jpg"
display_image: false  # change this to true to display the image below the banner 
---

# Introduction
<p class="intro"><span class="dropcap">L</span>anding a job fresh out of college can be a tough feat, especially for those trying to find a job in Los Angeles, California. Born and raised in and around the city, I have always planned on returning to the Los Angeles area after college. However, what I did not consider while drawing up this plan, was the current state of the job market. </p>

### Job Outlook for 2024
Position openings for newly-graduated students have on average, been cut in half since 2023, and the average time needed to acquire a job lies around [6 months](https://www.linkedin.com/pulse/job-outlook-class-2024-getting-college-grads-hired-7npce/). Many employers are now also requiring more [experience](https://nextgreatstep.com/should-college-grads-fake-it-until-they-make-it/) than the average college graduate currently has, such as problem-solving skills, business communication, and leadership. 

### Motivation 
As a student quickly approaching her graduation date, I have taken a strong interest in determining for myself what jobs in my field are currently available in Los Angeles. For this blog, I have chosen to scrape job data for Data Scientist positions from Indeed because it is one of the most popular online job boards throughout the country. In this post, I will explain how and why I was able to use Indeed's website to gather valuable job data in Los Angeles.

# Acquiring the Data

### Is This Data Free?

The short answer is [yes](https://www.octoparse.com/blog/how-to-scrape-indeed-job-posting)! Any information that can be attained from their API is allowed to be scraped. After researching the [Indeed API](https://docs.indeed.com/authorization?&aceid=&kw=adwords_c_9099621460_15516767951_0_0_pmax&sid=us_googconthajpmax-_c__g_9029857_gclid$_CjwKCAjwnv-vBhBdEiwABCYQAyn4D7OoUEYp552th-4b5uSocahCW9RYp4xqVSJ_BKgjCXaRYRMdfhoCmhEQAvD_BwE&gad_source=1&gclid=CjwKCAjwnv-vBhBdEiwABCYQAyn4D7OoUEYp552th-4b5uSocahCW9RYp4xqVSJ_BKgjCXaRYRMdfhoCmhEQAvD_BwE&gclsrc=aw.ds), it turns out a lot of work goes into using the API, and creating your own scraper is much easier to do. As long as you are using public job data (no personal info) and have legal uses for the data, there should be no problem scraping what you need. 

### My Data Collection Toolbox

![Figure]({{site.url}}/{{site.baseurl}}/assets/img/toolbox.png)

In Python, I used the `webdriver` from the Selenium package to launch Chrome, which is the browser I am most familiar with using. I additionally used Numpy and Pandas which are also Python libraries to conduct EDA on my data once it was gathered. 

### Building a Working Web-Scraper

1. Find the url of the site you would like to scrape. For this blog, I used Indeed, and narrowed my job search to [within 25 miles of Los Angeles](https://www.indeed.com/jobs?q=Data+Science&l=Los+Angeles%2C+CA&vjk=74cadd03cb0dd918). I could then use `webdriver.Chrome()` to launch Chrome.
{%- highlight python -%}
url = 'https://www.indeed.com/jobs?q=Data+Science&l=Los+Angeles%2C+CA&vjk=74cadd03cb0dd918'
driver = webdriver.Chrome(service=ChromeService(ChromeDriverManager().install()))
driver.get(url)
{%- endhighlight -%}



https://www.blog.datahut.co/post/scrape-indeed-using-selenium-and-beautifulsoup