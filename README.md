## Introduction
This is a Reddit scraper that can be used to scrap subreddits and comments from submissions.
The library we use in Python is PRAW and you will need your own reddit account and API credentials for PRAW.
This scaper will store the following information:
* post_id
* subreddit_name
* post_title
* post_body
* visual_content_url
* srape_time
* post_author
* post_publish_date
* post_comments

## Step One: Getting Reddit API credentials. 
Reddit is an American social news aggregation, web content rating, and discussion website whereas subreddit is a powerful network of niche communities that collectively define reddit. To scrap reddit submissions, you would need to get Reddit API credentials. 

To get the authentication information, you need to create a reddit app from https://www.reddit.com/prefs/apps.
You need to firstly click **create app** or **create another app**.
![Alt text](Instruction_picture/createApp.png)
![Alt text](Instruction_picture/CreateAppInfo.png)
![Alt text](Instruction_picture/Reddit_Credentials.png)

This will open a form where you need to fill in the app's name, description(optional) and the redirect uri. For the redirect uri you should write: http://localhost:8080

After pressing "create app", you can find the authentication information needed to create the **praw.Reddit** instance. You should save the **personal use script**, **secret key** and **user agent** to somewhere safe.

## Step Two: Getting Started
Praw can be installed using pip. After installing, you can import Praw by: <br />
`import praw` <br /> 
`import pandas as pd` <br />
We are going to store the results into Sqlite3 database after scraping the posts, thus we also need to import the following library: <br />
`import sqlite3` <br />
`import sqlalchemy as db` <br />
`from sqlalchemy import create_engine` <br />

## Step Three: Run the following 3 functions:
#### The "get_reddit" function:
* Store the credential informations as **client_id**, **client_secret**, **user_agent** and set up reddit for later use <br />

#### The "get_posts_redults" function:
* Used to get the scrap result and put it into a pandas dataframe
* This function will scrap results with `get_posts_result` by puting the reddit, keyword indicating the subreddit you want to scrap, and the maximum posts you need as parameter, and then put the scrap result into a pandas dataframe
* Here we only scrap those posts with visual content. We use the `post.is_self` to check whether a post only include text. <br />

#### The "transform_posts_results" function:
* Used to transform the data type of the dataframe into the type that Sqlite3 accept

## Step Four: Run the main function:
#### The main function contains code that export the pandas dataframe into Sqlite3 database table
You need to change the **credit_id**,  **client_secret**, **user_agent** information to your own credential information; <br />
You can set the reddit keyword you want to scrap and the maximun posts you need in the "get_posts_results" function; <br />
After converting the column data type into data type that SQL database can accept, we can now export the pandas dataframe to sqlite3 with sqlalchemy. <br />
* SQLite is a relational database management system contained in a C library
* Sqlalchemy is an open-source SQL toolkit and object-relational mapper for the Python programming language released under the MIT License. We can use SQLAlchemy to create a connection to a new SQLite database, which will be store with a name that we defined.

Then we can export the pandas dataframe (here we name it "reddit_shoes") into database named "reddit_shoes" (you can define your own database name): <br />
`engine = create_engine('sqlite:///reddit_shoes.db',echo=True)` <br />
`sqlite_connection = engine.connect()` <br />
`sqlite_table="reddit_shoes"`<br />
`posts_df.to_sql(sqlite_table,sqlite_connection,if_exists='replace')`<br />

## Step Five: Viewing scrap table from command line
If you want to view the table from the shell, you can type the following to check out the database. <br />
Here the name of our database is "reddit_shoes" <br />
`open reddit_shoes.db` <br />

You can also check out all the tables from this database using the following: <br />
`.tables`<br />

Then you can see the table content with this command: <br />
`select * from reddit_shoes;`







