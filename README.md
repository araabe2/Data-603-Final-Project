# Data-603-Final-Project

# Readme

This project originally had three goals:

   **Goal 1:** Use information from a variety of sources to create a database of tokens of one-, two-, and three-word combinations in order to identify what any word's average usage is, as well as how much it varies from that usage.
    
   **Goal 2:** By querying the database, allow users to see what a prospective review's suggested rating would be, as well as identify what word choices are effecting the results.
   
   **Goal 3:** By querying the database, allow users to provide a grouping of reviews that can suggest the overall satisfaction of the product by unifying the reviews and providing an average scoring.  The idea was that this could be done in situations where no pre-provided 5-star system is already in place, such as in book reviews or in data suspected to be biased.  An additional hope would be to provide the most common "unusual" words as a method of providing the user feedback on what reviewers thought were relevant, i.e. "quality", "bent immediately", or "fast service".
   
   > **Only goals 1 and 2 were accomplished.**

This program was **mostly written in Databricks**, where the database itself still lives, using **Spark** and **Python**.  The database is approximately 7gb large, so it is a pain to submit it, reupload it, run it from another source, etc.  It is also, unfortunately, corrupted in certain areas due to Databricks session timeout length when not receiving commands sometimes occurring before a write to the delta lake had completed, ignoring the rest of what was requested to be completed.  On a positive note, this only seems to have effected the averages and variances for the Yelp categories, and the goals of this project seem to be properly completed despite that.

> Link to the Databricks version:  
https://databricks-prod-cloudfront.cloud.databricks.com/public/4027ec902e239c93eaaa8714f173bcfc/3515981017084649/2111837125975741/7760959321468332/latest.html (should be live for 6 months)  
Most of the visualizations had to be removed in order to make the database sharable, due to size constraints.

> Link to separate Databricks documents specifically that will run the visualizations:  
Part 1: https://databricks-prod-cloudfront.cloud.databricks.com/public/4027ec902e239c93eaaa8714f173bcfc/3515981017084649/1936216346680663/7760959321468332/latest.html  
Part 2: https://databricks-prod-cloudfront.cloud.databricks.com/public/4027ec902e239c93eaaa8714f173bcfc/3515981017084649/1936216346680716/7760959321468332/latest.html  
Part 3: https://databricks-prod-cloudfront.cloud.databricks.com/public/4027ec902e239c93eaaa8714f173bcfc/3515981017084649/1936216346680746/7760959321468332/latest.html

**I don't really have the ability to grant any exploration of the database from here** due to the nature of it being a spark delta-table stored in the Databricks dbfs.  With enough elbow grease, the database could be reimported to a known location (ideally one with more resources than a free Databricks account receives) and the appropriate variables altered.

**Data sources are:**

>https://www.kaggle.com/datasets/promptcloud/walmart-product-reviews-dataset?resource=download

>https://www.kaggle.com/datasets/prakharrathi25/google-play-store-reviews

>https://www.kaggle.com/datasets/yelp-dataset/yelp-dataset?select=yelp_academic_dataset_review.json (required a splitter to be imported to Databricks, ended up being split into 35 parts)

>https://www.kaggle.com/datasets/bittlingmayer/amazonreviews?select=test.ft.txt.bz2 (ended up just being used for testing purposes)

There is a lot here that, if I were to do it again, I would attempt to do it differently.  Loading into the database in the first place ended up taking about a week of continual loading, which I *believe* is caused by me doing it serially and not actually utilizing the main benefits of Spark.  I also ran into a problem where I couldn't make a nested array of tokens like I wanted to, which I did not end up having time to figure out, but which, I suspect, was the main lynchpin to solving my serial vs parallel problem.  Also, database lookup, despite using Spark SQL, is the slowest part of the process of generating stars for the reviews.  This is probably due to the size of the database, but it would be good to experiment with fixing that, since for each review, it currently takes a minute or so to get the results.
