## 1.1 Crawler File Documentation

---

The crawlercode.py is inclusive of a parent abstract crawler class, several social media subclasses and testing unit class.
Similar to this markdown document, there are documemntations in place for each class. It can be seen when you include help(classname) in the code.
---

### 1.1.1 Crawler Abstract class

---

This function is an abstract crawler class. It has the shared attributes defined within all the crawlers and abstract functions that will be defined properly in the individual subclasses.

### 1.1.1.1 __init__

Description:

This is where the attributes of the parent class is defined. It takes in the parameters required to initiate the crawler which is the topic and dataframe.
Self.data is empty as this is dependent on the aunthenticate functions in each subclass

Parameters:

self
topic(str): the topic/hashtag to be crawled
df(dataframe): empty dataframe for the crawler with only headers defined.

Returns:
None

### 1.1.1.2 crawldatatop

Description:

This is an abstract function that crawls the top data which is used by all the subclasses. This is left as abstract as each crawler sub class has different attributes that they crawl

### 1.1.1.3 crawldatatop

Description:

This is an abstract function that saves the dataframes into CSV. Naming convention is defined under each subclasses 

### 1.1.1.4 authenticate

Description:

This is an abstract function to authenticate into each social media. However, not all crawlers require authentication hence this can be used for pulling html and saving to self.data

---

### 1.1.2 SubClass: Reddit Crawler

---

Parameter:

crawleddata(class): passes the parent class

### 1.1.2.1 authenticate

Description:

This functions authenticates into a valid reddit user with the specified id,secret,user_agent,username,password. The valid authentication is saved into the self.data attribute so that the whole session is authenticated by referring to this self.data

Parameters:
self

Returns:
self.data: returns the authenticated log in and saves into the self.data attribute

```Python
    #uses praw which is a Reddit API for crawling
    #https://www.reddit.com/prefs/app can be used to create your identification tokens
    self.data = praw.Reddit(client_id='1Wbphu7sZiWpfg', client_secret='8SiX9MqF6468B9-8zTNBbAr3AZiAMg',
                                user_agent='dengueapp', username='assignmentproj', password='Password123')
```

### 1.1.2.2 crawldatatop


Description:

This functions crawls the top reddit based on the self.topic that is specified during the creation of reddit instance. The crawled data is added to the originally empty dataframe based on the specified header.

Parameters:
self

Returns:
self.df: returns the updated dataframe with new crawled reddit entries

### 1.1.2.3 crawldatasubtopic

Description:

This functions crawls the reddit subtopic within the specified self.topic that is specified during the creation of reddit instance. The crawled data is added to the originally empty dataframe based on the specified header. Not implemented for use but can be an alternative to crawldatatop, if a specific subtopic would like to crawled within the self.topic

Parameters:
self
subtopic(str): takes in the value of the subtopic within a topic

Returns:
self.df: returns the updated dataframe with new crawled reddit entries

### 1.1.2.4 getsentiment

Description:

This functions adds a new column to the dataframe named 'Sentiment'. The Sentiment will check on the polarity of the body.
A positive sentiment value = Positive Comment
Zero sentiment value = Nuetral Comment
A negative sentiment value = Negative Comment


Parameters:
self

Returns:
self.df: returns the updated dataframe with new crawled reddit entries 

```Python
    #This line applies a function to the body column to get the polairty using the TextBlob libary and saves it into a new column called sentiment
    self.df['Sentiment'] = self.df['Body'].apply(lambda x: TextBlob(x).sentiment.polarity)
```

### 1.1.2.4 cleandata 

Description:

This functions cleans up the dataframe by dropping irrelevant columns for easier analysis. It also creates 'Year' and 'Month' columns in case any time serieswould be used in the analysis.

Parameters:
self

Returns:
self.df: returns the updated dataframe with new crawled reddit entries

### 1.1.2.4 saveCV

Description:
This functions saves the dataframes into csv for use for analysis and for the front end. Local file storage is used as the program's database. It saves it in respective folders, if folder is not found, it will create a folder.

Parameters:
self

Returns:
None

---

### 1.1.3 SubClass: Stack Crawler

---

Parameters:
crawleddata(class): passes the parent class

### 1.1.3.1 authenticate

Description:

Stack does not need proper authentication.
This functions uses beautifulsoup4 to get the whole html page and save the data into self.data.

Parameters:
self

Returns:
self.data: returns the authenticated log in and saves into the self.data attribute

```Python
    #uses bs4 library to crawl the contents of the page
    site = requests.get('https://stackoverflow.com/questions/tagged/'+self.topic); 
    #soup holds the whole html that is crawled from bs4
    soup = BeautifulSoup(site.text,"html.parser")
    self.data = soup.select(".question-summary")
```

### 1.1.3.2 crawldatatop

Description:

This function seperates each entry within the whole html in self.data and splits it into the different rows and columns of the dataframe. The crawled data is added to the originally empty dataframe based on the specified header.

Parameters:
self

Returns:
self.df: returns the updated dataframe with new crawled stack entries

### 1.1.3.3 voterank

Description:
This function ranks the stack overflow entries based on the popularity of the views

Parameters:
self

Returns:
self.df: returns the updated dataframe with new crawled stack entries

```Python
    #A function is applied to the view column to change it to numeric from string and then sorted to base on popularity
    self.df["Views"]= self.df['Views'].apply(lambda x: x[:2])
    self.df["Views"] = pd.to_numeric(self.df["Views"])
    self.df = self.df.sort_values(by='Views', ascending=False)
```

### 1.1.3.4 saveCV

Description:

This functions saves the dataframes into csv for use for analysis and for the front end.
Local file storage is used as the program's database.
It saves it in respective folders, if folder is not found, it will create a folder.

Parameters:
self

Returns:
None

---

### 1.1.4 SubClass: GitHub Crawler

---

Parameters:
crawleddata(class): passes the parent class

### 1.1.4.1 authenticate

Description:

This functions authenticates into a valid stack overflow user with the specified access token. The valid authentication is saved into the self.data attribute so that the whole session is authenticated by referring to this self.data

Parameters:
self

Returns:
self.data: returns the authenticated log in and saves into the self.data attribute

### 1.1.4.2 crawldatatop

Description:

This functions crawls the git hub entries based on the self.topic that is specified during the creation of github instance.
The crawled data is added to the originally empty dataframe based on the specified header.
Another column is created as well: Other Programming Languages URL to access all the languages present in the repository
Calculation for the percentage of all the languages present in the repository is also done.

Parameters:
self

Returns:
self.df: returns the updated dataframe with new crawled reddit entries

### 1.1.4.3 saveCV

Description:
This functions saves the dataframes into csv for use for analysis and for the front end.
Local file storage is used as the program's database.
It saves it in respective folders, if folder is not found, it will create a folder.

Parameters:
self

Returns:
None

---

### 1.1.5 SubClass: Twitter Crawler

---

Parameters:
crawleddata(class): passes the parent class

### 1.1.5.1 authenticate

Description:

This functions authenticates into a valid twitter user with the specified Handler and access token.
The valid authentication is saved into the self.data attribute so that the whole session is authenticated by referring to this self.data

Parameters:
self

Returns:
self.data: returns the authenticated log in and saves into the self.data attribute

### 1.1.5.2 crawldatatop

Description:
This function crawls the top tweets that matches the hashtag (self.topic). The crawled data is added to the originally empty dataframe based on the specified header.

Parameters:
self

Returns:
self.df: returns the updated dataframe with new crawled twitter entries

### 1.1.5.3 crawldatarecent

Description:

This function crawls the recent tweets that matches the hashtag (self.topic). The crawled data is added to the originally empty dataframe based on the specified header.

Parameters:
self

Returns:
self.df: returns the updated dataframe with new crawled twitter entries


### 1.1.5.4 saveCV

Description:
This functions saves the dataframes into csv for use for analysis and for the front end.
Local file storage is used as the program's database.
It saves it in respective folders, if folder is not found, it will create a folder.

Parameters:
self

Returns:
None


---

### 1.1.6 Test Cases

---

This class has all the functions that performs unit testing. Unit testing is done to ensure program runs smoothly with no loopholes.

### 1.1.6.1 Test Case 1

Description:

Test if crawl time is float value & if subtraction method is correct

### 1.1.6.2 Test Case 2

Description:

Test if crawl time is lesser than 5 minutes

### 1.1.6.3 Test Case 3

Description:

Test if CSV file is created and stored in os.path
        
### 1.1.6.3 Test Case 4

Description:

Test if folder contain correct number of files crawled 

---

### 1.1.6 Task

---

Description:

This function is made to allow main function to be clean. Hence, all the task that needs to be run is within this class and function.
Dataframe for each crawler is predefined here.
Array of topics to be crawled is also defined here.
Instances of each crawler objects are defined here as well.

---

### 1.1.7 Main Program

---

The main program starts the timer and call the task class to run the various crawlers. Scheduler is also tasked to run the program every 6 minutes. 
