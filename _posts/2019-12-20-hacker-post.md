---
title: "Exploring Hacker News Post"
date: 2019-12-20
tags: [data wrangling, data science, messy data, data science projects]
header:
  image: "/images/perceptron/percept.jpg"
excerpt: "Data Wrangling, Data Science, Messy Data"
mathjax: "true"
---

```python
# Exploring Hacker News Posts

Hacker News is a site started by the startup incubator Y Combinator, where user-submitted stories (known as "posts") are voted and commented upon, similar to reddit. Hacker News is extremely popular in technology and startup circles, and posts that make it to the top of Hacker News' listings can get hundreds of thousands of visitors as a result.

You can find the data set here, but note that it has been reduced from almost 300,000 rows to approximately 20,000 rows by removing all submissions that did not receive any comments, and then randomly sampling from the remaining submissions.

**id:** The unique identifier from Hacker News for the post  
**title:** The title of the post  
**url:** The URL that the posts links to, if it the post has a URL  
**num_points:** The number of points the post acquired, calculated as the total number of upvotes minus the total number of downvotes  
**num_comments:** The number of comments that were made on the post  
**author:** The username of the person who submitted the post  
**created_at:** The date and time at which the post was submitted  

We're specifically interested in posts whose titles begin with either Ask HN or Show HN. Users submit Ask HN posts to ask the Hacker News community a specific question.  

Likewise, users submit Show HN posts to show the Hacker News community a project, product, or just generally something interesting.  

## Importing the Dataset


```python
import pandas as pd


hn = pd.read_csv("hacker_news.csv")

# Displaying the first 5 rows

hn.head(6)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>id</th>
      <th>title</th>
      <th>url</th>
      <th>num_points</th>
      <th>num_comments</th>
      <th>author</th>
      <th>created_at</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>12224879</td>
      <td>Interactive Dynamic Video</td>
      <td>http://www.interactivedynamicvideo.com/</td>
      <td>386</td>
      <td>52</td>
      <td>ne0phyte</td>
      <td>8/4/2016 11:52</td>
    </tr>
    <tr>
      <th>1</th>
      <td>10975351</td>
      <td>How to Use Open Source and Shut the Fuck Up at...</td>
      <td>http://hueniverse.com/2016/01/26/how-to-use-op...</td>
      <td>39</td>
      <td>10</td>
      <td>josep2</td>
      <td>1/26/2016 19:30</td>
    </tr>
    <tr>
      <th>2</th>
      <td>11964716</td>
      <td>Florida DJs May Face Felony for April Fools' W...</td>
      <td>http://www.thewire.com/entertainment/2013/04/f...</td>
      <td>2</td>
      <td>1</td>
      <td>vezycash</td>
      <td>6/23/2016 22:20</td>
    </tr>
    <tr>
      <th>3</th>
      <td>11919867</td>
      <td>Technology ventures: From Idea to Enterprise</td>
      <td>https://www.amazon.com/Technology-Ventures-Ent...</td>
      <td>3</td>
      <td>1</td>
      <td>hswarna</td>
      <td>6/17/2016 0:01</td>
    </tr>
    <tr>
      <th>4</th>
      <td>10301696</td>
      <td>Note by Note: The Making of Steinway L1037 (2007)</td>
      <td>http://www.nytimes.com/2007/11/07/movies/07ste...</td>
      <td>8</td>
      <td>2</td>
      <td>walterbell</td>
      <td>9/30/2015 4:12</td>
    </tr>
    <tr>
      <th>5</th>
      <td>10482257</td>
      <td>Title II kills investment? Comcast and other I...</td>
      <td>http://arstechnica.com/business/2015/10/comcas...</td>
      <td>53</td>
      <td>22</td>
      <td>Deinos</td>
      <td>10/31/2015 9:48</td>
    </tr>
  </tbody>
</table>
</div>



## Subsetting the Dataset for "ask hn" and "show hn" posts 


```python
# Converting the title column into lower for subsetting
hn['title'] = hn['title'].str.lower()

# Subsetting the ask hn posts
boolean_ask = hn['title'].str.startswith('ask hn')
hn_ask_posts = hn[boolean_ask]
hn_ask_posts.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>id</th>
      <th>title</th>
      <th>url</th>
      <th>num_points</th>
      <th>num_comments</th>
      <th>author</th>
      <th>created_at</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>7</th>
      <td>12296411</td>
      <td>ask hn: how to improve my personal website?</td>
      <td>NaN</td>
      <td>2</td>
      <td>6</td>
      <td>ahmedbaracat</td>
      <td>8/16/2016 9:55</td>
    </tr>
    <tr>
      <th>17</th>
      <td>10610020</td>
      <td>ask hn: am i the only one outraged by twitter ...</td>
      <td>NaN</td>
      <td>28</td>
      <td>29</td>
      <td>tkfx</td>
      <td>11/22/2015 13:43</td>
    </tr>
    <tr>
      <th>22</th>
      <td>11610310</td>
      <td>ask hn: aby recent changes to css that broke m...</td>
      <td>NaN</td>
      <td>1</td>
      <td>1</td>
      <td>polskibus</td>
      <td>5/2/2016 10:14</td>
    </tr>
    <tr>
      <th>30</th>
      <td>12210105</td>
      <td>ask hn: looking for employee #3 how do i do it?</td>
      <td>NaN</td>
      <td>1</td>
      <td>3</td>
      <td>sph130</td>
      <td>8/2/2016 14:20</td>
    </tr>
    <tr>
      <th>31</th>
      <td>10394168</td>
      <td>ask hn: someone offered to buy my browser exte...</td>
      <td>NaN</td>
      <td>28</td>
      <td>17</td>
      <td>roykolak</td>
      <td>10/15/2015 16:38</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Subsetting the show hn posts
boolean_show = hn['title'].str.startswith('show hn')
hn_show_posts = hn[boolean_show]
hn_show_posts.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>id</th>
      <th>title</th>
      <th>url</th>
      <th>num_points</th>
      <th>num_comments</th>
      <th>author</th>
      <th>created_at</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>13</th>
      <td>10627194</td>
      <td>show hn: wio link  esp8266 based web of things...</td>
      <td>https://iot.seeed.cc</td>
      <td>26</td>
      <td>22</td>
      <td>kfihihc</td>
      <td>11/25/2015 14:03</td>
    </tr>
    <tr>
      <th>39</th>
      <td>10646440</td>
      <td>show hn: something pointless i made</td>
      <td>http://dn.ht/picklecat/</td>
      <td>747</td>
      <td>102</td>
      <td>dhotson</td>
      <td>11/29/2015 22:46</td>
    </tr>
    <tr>
      <th>46</th>
      <td>11590768</td>
      <td>show hn: shanhu.io, a programming playground p...</td>
      <td>https://shanhu.io</td>
      <td>1</td>
      <td>1</td>
      <td>h8liu</td>
      <td>4/28/2016 18:05</td>
    </tr>
    <tr>
      <th>84</th>
      <td>12178806</td>
      <td>show hn: webscope  easy way for web developers...</td>
      <td>http://webscopeapp.com</td>
      <td>3</td>
      <td>3</td>
      <td>fastbrick</td>
      <td>7/28/2016 7:11</td>
    </tr>
    <tr>
      <th>97</th>
      <td>10872799</td>
      <td>show hn: geoscreenshot  easily test geo-ip bas...</td>
      <td>https://www.geoscreenshot.com/</td>
      <td>1</td>
      <td>9</td>
      <td>kpsychwave</td>
      <td>1/9/2016 20:45</td>
    </tr>
  </tbody>
</table>
</div>



## Determine if ask posts or show posts receive more comments on average


```python
# Ask Comments on average
total_ask_comments = hn_ask_posts['num_comments'].sum() / len(hn_ask_posts)
print("Total number of average comments on ask posts are: ",total_ask_comments)

# show Comments on average
total_show_comments = hn_show_posts['num_comments'].sum() / len(hn_show_posts)
print("Total number of average comments on show posts are: ",total_show_comments)


```

    Total number of average comments on ask posts are:  14.038417431192661
    Total number of average comments on show posts are:  10.31669535283993


On average, ask posts in our sample receive approximately 14 comments, whereas show posts receive approximately 10. Since ask posts are more likely to receive comments, we'll focus our remaining analysis just on these posts.  


## Calculate the amount of ask posts created per hour, along with the total amount of comments 


```python
import datetime as dt

result_list = hn_ask_posts[["num_comments","created_at"]]

result_list['datetime'] = pd.to_datetime(result_list['created_at'])

result_list['freq'] = result_list['datetime'].dt.strftime('%H')

result_list.head()
count_by_hour = result_list.groupby('freq').count()
count_by_hour = count_by_hour.drop(["created_at","datetime"], axis = 1)
#count_by_hour = count_by_hour.rename(columns = {'num_comments':'count'}, inplace = True)
count_by_hour
```

    /Users/Aniket/anaconda3/lib/python3.7/site-packages/ipykernel_launcher.py:5: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-view-versus-copy
      """
    /Users/Aniket/anaconda3/lib/python3.7/site-packages/ipykernel_launcher.py:8: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-view-versus-copy
      





<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>num_comments</th>
    </tr>
    <tr>
      <th>freq</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>00</th>
      <td>55</td>
    </tr>
    <tr>
      <th>01</th>
      <td>60</td>
    </tr>
    <tr>
      <th>02</th>
      <td>58</td>
    </tr>
    <tr>
      <th>03</th>
      <td>54</td>
    </tr>
    <tr>
      <th>04</th>
      <td>47</td>
    </tr>
    <tr>
      <th>05</th>
      <td>46</td>
    </tr>
    <tr>
      <th>06</th>
      <td>44</td>
    </tr>
    <tr>
      <th>07</th>
      <td>34</td>
    </tr>
    <tr>
      <th>08</th>
      <td>48</td>
    </tr>
    <tr>
      <th>09</th>
      <td>45</td>
    </tr>
    <tr>
      <th>10</th>
      <td>59</td>
    </tr>
    <tr>
      <th>11</th>
      <td>58</td>
    </tr>
    <tr>
      <th>12</th>
      <td>73</td>
    </tr>
    <tr>
      <th>13</th>
      <td>85</td>
    </tr>
    <tr>
      <th>14</th>
      <td>107</td>
    </tr>
    <tr>
      <th>15</th>
      <td>116</td>
    </tr>
    <tr>
      <th>16</th>
      <td>108</td>
    </tr>
    <tr>
      <th>17</th>
      <td>100</td>
    </tr>
    <tr>
      <th>18</th>
      <td>109</td>
    </tr>
    <tr>
      <th>19</th>
      <td>110</td>
    </tr>
    <tr>
      <th>20</th>
      <td>80</td>
    </tr>
    <tr>
      <th>21</th>
      <td>109</td>
    </tr>
    <tr>
      <th>22</th>
      <td>71</td>
    </tr>
    <tr>
      <th>23</th>
      <td>68</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Comments by Hour
comments_by_hour = result_list.groupby('freq').sum()
comments_by_hour = comments_by_hour.sort_values(by = 'num_comments')
comments_by_hour
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>num_comments</th>
    </tr>
    <tr>
      <th>freq</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>09</th>
      <td>251</td>
    </tr>
    <tr>
      <th>07</th>
      <td>267</td>
    </tr>
    <tr>
      <th>04</th>
      <td>337</td>
    </tr>
    <tr>
      <th>06</th>
      <td>397</td>
    </tr>
    <tr>
      <th>03</th>
      <td>421</td>
    </tr>
    <tr>
      <th>00</th>
      <td>447</td>
    </tr>
    <tr>
      <th>05</th>
      <td>464</td>
    </tr>
    <tr>
      <th>22</th>
      <td>479</td>
    </tr>
    <tr>
      <th>08</th>
      <td>492</td>
    </tr>
    <tr>
      <th>23</th>
      <td>543</td>
    </tr>
    <tr>
      <th>11</th>
      <td>641</td>
    </tr>
    <tr>
      <th>01</th>
      <td>683</td>
    </tr>
    <tr>
      <th>12</th>
      <td>687</td>
    </tr>
    <tr>
      <th>10</th>
      <td>793</td>
    </tr>
    <tr>
      <th>17</th>
      <td>1146</td>
    </tr>
    <tr>
      <th>19</th>
      <td>1188</td>
    </tr>
    <tr>
      <th>13</th>
      <td>1253</td>
    </tr>
    <tr>
      <th>02</th>
      <td>1381</td>
    </tr>
    <tr>
      <th>14</th>
      <td>1416</td>
    </tr>
    <tr>
      <th>18</th>
      <td>1439</td>
    </tr>
    <tr>
      <th>20</th>
      <td>1722</td>
    </tr>
    <tr>
      <th>21</th>
      <td>1745</td>
    </tr>
    <tr>
      <th>16</th>
      <td>1814</td>
    </tr>
    <tr>
      <th>15</th>
      <td>4477</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Counts by hour
count_by_hour = result_list.groupby('freq').count()
count_by_hour = count_by_hour.drop(["created_at","datetime"], axis = 1)
count_by_hour = count_by_hour.sort_values(by = 'num_comments')
count_by_hour
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>num_comments</th>
    </tr>
    <tr>
      <th>freq</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>07</th>
      <td>34</td>
    </tr>
    <tr>
      <th>06</th>
      <td>44</td>
    </tr>
    <tr>
      <th>09</th>
      <td>45</td>
    </tr>
    <tr>
      <th>05</th>
      <td>46</td>
    </tr>
    <tr>
      <th>04</th>
      <td>47</td>
    </tr>
    <tr>
      <th>08</th>
      <td>48</td>
    </tr>
    <tr>
      <th>03</th>
      <td>54</td>
    </tr>
    <tr>
      <th>00</th>
      <td>55</td>
    </tr>
    <tr>
      <th>11</th>
      <td>58</td>
    </tr>
    <tr>
      <th>02</th>
      <td>58</td>
    </tr>
    <tr>
      <th>10</th>
      <td>59</td>
    </tr>
    <tr>
      <th>01</th>
      <td>60</td>
    </tr>
    <tr>
      <th>23</th>
      <td>68</td>
    </tr>
    <tr>
      <th>22</th>
      <td>71</td>
    </tr>
    <tr>
      <th>12</th>
      <td>73</td>
    </tr>
    <tr>
      <th>20</th>
      <td>80</td>
    </tr>
    <tr>
      <th>13</th>
      <td>85</td>
    </tr>
    <tr>
      <th>17</th>
      <td>100</td>
    </tr>
    <tr>
      <th>14</th>
      <td>107</td>
    </tr>
    <tr>
      <th>16</th>
      <td>108</td>
    </tr>
    <tr>
      <th>18</th>
      <td>109</td>
    </tr>
    <tr>
      <th>21</th>
      <td>109</td>
    </tr>
    <tr>
      <th>19</th>
      <td>110</td>
    </tr>
    <tr>
      <th>15</th>
      <td>116</td>
    </tr>
  </tbody>
</table>
</div>



## Calculate the average number of comments per post for posts created during each hour of the day


```python
avg_by_hour = comments_by_hour / count_by_hour
avg_by_hour = avg_by_hour.sort_values(by = 'num_comments')
avg_by_hour
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>num_comments</th>
    </tr>
    <tr>
      <th>freq</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>09</th>
      <td>5.577778</td>
    </tr>
    <tr>
      <th>22</th>
      <td>6.746479</td>
    </tr>
    <tr>
      <th>04</th>
      <td>7.170213</td>
    </tr>
    <tr>
      <th>03</th>
      <td>7.796296</td>
    </tr>
    <tr>
      <th>07</th>
      <td>7.852941</td>
    </tr>
    <tr>
      <th>23</th>
      <td>7.985294</td>
    </tr>
    <tr>
      <th>00</th>
      <td>8.127273</td>
    </tr>
    <tr>
      <th>06</th>
      <td>9.022727</td>
    </tr>
    <tr>
      <th>12</th>
      <td>9.410959</td>
    </tr>
    <tr>
      <th>05</th>
      <td>10.086957</td>
    </tr>
    <tr>
      <th>08</th>
      <td>10.250000</td>
    </tr>
    <tr>
      <th>19</th>
      <td>10.800000</td>
    </tr>
    <tr>
      <th>11</th>
      <td>11.051724</td>
    </tr>
    <tr>
      <th>01</th>
      <td>11.383333</td>
    </tr>
    <tr>
      <th>17</th>
      <td>11.460000</td>
    </tr>
    <tr>
      <th>18</th>
      <td>13.201835</td>
    </tr>
    <tr>
      <th>14</th>
      <td>13.233645</td>
    </tr>
    <tr>
      <th>10</th>
      <td>13.440678</td>
    </tr>
    <tr>
      <th>13</th>
      <td>14.741176</td>
    </tr>
    <tr>
      <th>21</th>
      <td>16.009174</td>
    </tr>
    <tr>
      <th>16</th>
      <td>16.796296</td>
    </tr>
    <tr>
      <th>20</th>
      <td>21.525000</td>
    </tr>
    <tr>
      <th>02</th>
      <td>23.810345</td>
    </tr>
    <tr>
      <th>15</th>
      <td>38.594828</td>
    </tr>
  </tbody>
</table>
</div>



The hour that receives the most comments per post on average is 15:00, with an average of 38.59 comments per post. There's about a 60% increase in the number of comments between the hours with the highest and second highest average number of comments.

According to the data set documentation, the timezone used is Eastern Time in the US. So, we could also write 15:00 as 3:00 pm est.

## Conclusion

In this project, we analyzed ask posts and show posts to determine which type of post and time receive the most comments on average. Based on our analysis, to maximize the amount of comments a post receives, we'd recommend the post be categorized as ask post and created between 15:00 and 16:00 (3:00 pm est - 4:00 pm est).

```