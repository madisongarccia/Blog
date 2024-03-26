---
layout: post
title:  Data Curation Blog Post
date: 2024-03-20
description: 
image: "/assets/img/LA_blog.jpg"
display_image: false  # change this to true to display the image below the banner 
---

# Introduction

***

<p class="intro"><span class="dropcap">L</span>anding a job fresh out of college can be a tough feat, especially for those trying to find a job in Los Angeles, California. Born and raised in and around the city, I have always planned on returning to the Los Angeles area after college. However, what I did not consider while drawing up this plan, was the current state of the job market. </p>

### Job Outlook for 2024
Position openings for newly-graduated students have on average, been cut in half since 2023, and the average time needed to acquire a job lies around [6 months](https://www.linkedin.com/pulse/job-outlook-class-2024-getting-college-grads-hired-7npce/). Many employers are now also requiring more [experience](https://nextgreatstep.com/should-college-grads-fake-it-until-they-make-it/) than the average college graduate currently has, such as problem-solving skills, business communication, and leadership. 

### Motivation 
As a student quickly approaching her graduation date, I have taken a strong interest in determining for myself what jobs in my field are currently available in Los Angeles. For this blog, I have chosen to scrape job data for Data Scientist positions from Indeed because it is one of the most popular online job boards throughout the country. In this post, I will explain how and why I was able to use Indeed's website to gather valuable job data in Los Angeles.

# Acquiring the Data

***

### Is This Data Free?

The short answer is [yes](https://www.octoparse.com/blog/how-to-scrape-indeed-job-posting)! Any information that can be attained from their API is allowed to be scraped. After researching the [Indeed API](https://docs.indeed.com/authorization?&aceid=&kw=adwords_c_9099621460_15516767951_0_0_pmax&sid=us_googconthajpmax-_c__g_9029857_gclid$_CjwKCAjwnv-vBhBdEiwABCYQAyn4D7OoUEYp552th-4b5uSocahCW9RYp4xqVSJ_BKgjCXaRYRMdfhoCmhEQAvD_BwE&gad_source=1&gclid=CjwKCAjwnv-vBhBdEiwABCYQAyn4D7OoUEYp552th-4b5uSocahCW9RYp4xqVSJ_BKgjCXaRYRMdfhoCmhEQAvD_BwE&gclsrc=aw.ds), it turns out a lot of work goes into using the API, and creating your own scraper is much easier to do. As long as you are using public job data (no personal info) and have legal uses for the data, there should be no problem scraping what you need. 

### My Data Collection Toolbox

![Figure]({{site.url}}/{{site.baseurl}}/assets/img/toolbox.png)

In Python, I used the `webdriver` from the Selenium package to launch Chrome, which is the browser I am most familiar with using. I additionally used Numpy and Pandas which are also Python libraries to conduct EDA on my data once it was gathered. 

### Building a Working Web-Scraper

1. Find the url of the site you would like to scrape

For this blog, I used Indeed, narrowed my job search to [within 25 miles of Los Angeles](https://www.indeed.com/jobs?q=Data+Science&l=Los+Angeles%2C+CA&vjk=74cadd03cb0dd918), and got the url below. I could then use `webdriver.Chrome()` to launch Chrome.
{%- highlight python -%}
url = 'https://www.indeed.com/jobs?q=Data+Science&l=Los+Angeles%2C+CA&vjk=74cadd03cb0dd918'
driver = webdriver.Chrome(service=ChromeService(ChromeDriverManager().install()))
driver.get(url)
{%- endhighlight -%}
More information on Selenium's WebDriver for debugging purposes can be found [here](https://www.selenium.dev/documentation/webdriver/).

2. Containers and Pagination

Before collecting specific job data, I need to find in the HTML where the [container](https://www.w3schools.com/w3css/w3css_containers.asp) for all the jobs is located. This way, I can locate within that container, all the individual job elements. This list of all job elements on a page can be used later in my code to iterate through. Below is an example of how I found both of these elements in my code.
{%- highlight python -%}
container = driver.find_element(By.XPATH, 
                                ".//div[contains(@id, 'mosaic-jobResults')]")
all_jobs = container.find_elements(By.XPATH, 
                                ".//div[contains(@class, 'job_seen_beacon')]")
{%- endhighlight -%}
Guide on [`.find_element(s)`](https://selenium-python.readthedocs.io/locating-elements.html)

The variable `all_jobs` should now be a list containing each job card on the first page. 

<figure>
  <img src="{{site.url}}/{{site.baseurl}}/assets/img/sample_card.png" alt="Description of image" style="width:300px;height:350px;">
  <figcaption>Sample Job Card</figcaption>
</figure>

After looking at all 15 jobs on the first page, I would also like my web-scraper to be able to click to the next page if applicable, and continue gathering information from multiple pages. To do this, I need a pagination variable and to find the next button. Pagination refers to the navigation bar at the bottom on websites that have multiple pages you can move between.

![Figure]({{site.url}}/{{site.baseurl}}/assets/img/navigation.png)

There should also be HTML code within the specific pagination tag that represents the next button you can click. We want to locate that, save it as a `next` button variable, and use `next.click()` to go to the next page. 

{%- highlight python -%}
pagination = driver.find_element(By.XPATH, ".//nav[contains(@role, 'navigation')]")
... Code To Collect Data on One Page ...
try:
    next = WebDriverWait(driver, 10).until(EC.element_to_be_clickable((By XPATH, ".//a[contains(@aria-label, 'Next Page')]")))
    next.click()
except:
    print(f'failed on page {page_num})
    break
{%- endhighlight -%}

At this point, we should have the general navigational tools, and job card information necessary to carry out our data extraction. 

3. What information would you like to gather?

For this particular dataset, I was most interested in finding what I could on the following categories:

* `Title` - Job Title
* `Company` - Company Name
* `Location` - City and State
* `Pay` - Salary Amount
* `Job_Description` - Job Description
* `When_Posted` - How long ago was this posted?
* `Rating` - Company Rating
* `Is_Hybrid_Remote` - Location Type (On-Site, Hybrid, Remote)

Most of this information was readily available on the job postings, but in order to gather this data, I needed to locate where each item would be found in the HTML code. 

{%- highlight python -%}
for job in all_jobs:
    title = job.find_element(By.XPATH, ".//h2[contains(@class, 'jobTitle')]").text
    try:
        location = job.find_element(By.XPATH, ".//div[contains(@class, 'company_location')]").text.split("\n")[1]
    except:
        location = job.find_element(By.XPATH, ".//div[contains(@class, 'company_location')]").text.split("\n")[0]
    company = job.find_element(By.XPATH, ".//div[contains(@class, 'company_location')]").text.split("\n")[0]
    when_posted= job.find_element(By.XPATH, ".//tr[contains(@class, 'underShelfFooter')]").text.split("\n")[-1].strip("More...")
    description = job.find_element(By.XPATH, ".//tr[contains(@class, 'underShelfFooter')]").text.split("\n")[0]
    try:
        string = job.find_element(By.XPATH, ".//div[contains(@class, 'salary-snippet-container')]").text
        listy = string.split()
        for element in listy:
            if "$" in element:
                pay = element
    except:
        pay = "NaN"
    job_titles.append(title)
    job_pays.append(pay)
    company_names.append(company)
    descriptions.append(description)
    company_locations.append(location)
    posted.append(when_posted)
{%- endhighlight -%}

https://www.blog.datahut.co/post/scrape-indeed-using-selenium-and-beautifulsoup