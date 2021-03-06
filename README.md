# Search Console BQ
### Getting data from Google Search Console into BigQuery with some readable Python code
In this single file of Python code, we build a function that downloads data from Google Search Console and sends it to Google BigQuery. We can loop through this function to grab lots of data for multiple properties with ease.

## Why use this code?
Many examples currently shared online are outdated and make the process of downloading data from Google Search Console overly complicated (in my humble opinion). The aim of this code is to simplify the process and make it easy for anyone with a little Python knowledge to apply.

Some highlights:
- Easily transferable; deploy anywhere, everything can be defined in one easy to read piece of code.
- Written in Python 3 (obviously, but many of the older code examples for Search Console are still in Python 2).
- Grab ALL your data, using the latest increased API limit and loops to ensure we capture everything!
- Easy to scale — simple to add in a new search console property.

## How do I use it?
Running through the notebook may help you get a better understanding of the code and what it is doing. Once you are happy and have made the required customizations to the code (adding your properties and authentication keys) you can deploy it. One option for deployment is to run the .py file on a Goole Cloud Virtual Machine, using a CRON trigger, this way you'll get your data into BigQuery every day without having to lift a finger!

## Files
`search_console_bq.py` contains working code with examples of looping over multiple properties, one should update the variables at the top before using.

`Search console BQ 2020.ipynb` is a Jupyter notebook containing all the code, a good starting point if you want to make any changes or run through the process interactively.

## Required Python packages
- pyarrow, for sending data to BigQuery: 
- Google auth libraries: `pip install --upgrade google-api-python-client google-auth-httplib2 google-auth-oauthlib`
- Google Cloud library: `pip install google-cloud`

**If you run the code and notice that I have forgotten a package from this list please open an issue and let me know!**

## Required technical steps:

- Create a Google Cloud project.
- Activate the Google Search Console API on your GCP project.
- Create a service account user, with access to write to BigQuery.
- Add this service account user to each Search Console property or properties that you want to download data from.
- Edit the search_console_bq.py file and set the parameters at the top of the code, populating them with your values.
- The code is designed to load all data to a single BigQuery table, appending each time. This table does not need to exist before the first run.
- The code will collect all data up to 100,000 rows, if you expect to exceed this number for any given data in Search Console then you should increase this number. This won't slow you down days that have far less data, because the loop will stop once the data chunk size is less than 100%.

## Loading into BigQuery
This code, by default, will append to a single BigQuery table with each new day’s data. This table can quickly become huge, I would recommend partitioning to ensure your table remains efficient (both cost and time) to access.

## Further considerations
At the time of writing Google takes about 2/3 days to process Search Console data, so a request for yesterday's numbers will yield no results. Consider this when deploying your code - you can, for example, deploy to collect data for three days ago, every day. Should Google ever speed this process up you can then adjust the code.

Read more details on this code and my approach on this Medium post:
https://medium.com/@singularbean/revisiting-google-search-console-data-into-google-bigquery-708a19e2f746
