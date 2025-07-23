# Dashboard for Tracking Influencer Campaigns

## Context:
HealthKart runs influencer campaigns across various social platforms (Instagram, YouTube, Twitter, etc.) to promote different products across multiple brands (e.g., MuscleBlaze, HKVitals, Gritzo). Influencers may be paid per post or per order.

For this we are to build a dashboard that shows:
- Campaign performance
- Incremental ROAS (Return on Ad Spend)
- Influencer insights
- Payout tracking

## Objective
To build a dashboard that can track and visualize the ROI of influencer campaigns.

## Process
1. Simulated the following datasets containing the respective columns using ChatGPT:
  - influencers: ID, name, category, gender, follower count, platform
  - posts: influencer_id, platform, date, URL, caption, reach, likes, comments
  - tracking_data: source, campaign, influencer_id, user_id, product, date, orders, revenue
  - payouts: influencer_id, basis (post/order), rate, orders, total_payout 

### Understanding the data:
Assumptions / data description:
1. influencers - This table holds metadata about the influencers participating in your campaign.

   500 unique influencers
   
   
| Column         | Data type | Description                                                              | Assumptions Made                                                                                 |
|----------------|-----------|--------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------|
| id             | int       | Unique numeric identifier for each influencer                            | Sequential IDs from 1 to 500                                                                     |
| name           | string    | Full name of the influencer                                              | Randomly generated using Faker                                                                   |
| category       | string    | The niche or content domain of the influencer (e.g., Fitness, Nutrition) | Randomly selected from a curated list: "Fitness", "Nutrition", "Wellness", "Lifestyle", "Yoga"   |
| gender         | string    | Gender of the influencer                                                 | Randomly selected from: "Male", "Female", "Other"                                                |
| follower_count | int       | Total followers the influencer has across their primary platform         | Random int between 5,000 and 1,000,000 to represent nano to macro influencers                    |
| platform       | string    | Primary social media platform used by the influencer                     | Randomly chosen from: "Instagram", "YouTube", "TikTok", "Twitter"                                |

2. posts - Each row represents a piece of content posted by an influencer on a platform.

   Number of rows - 10000

| Column        | Data type | Description                            | Assumptions Made                                                                 |
|---------------|-----------|----------------------------------------|----------------------------------------------------------------------------------|
| influencer_id | int       | Foreign key referencing influencers.id | Ensures referential integrity — matches one of the 500 influencers               |
| platform      | string    | Platform on which the post was made    | Matches the influencer's preferred platform or chosen randomly for variety       |
| date          | date      | Date when the post was published       | Random date within the last 90 days (-90d to today)                              |
| URL           | string    | Simulated link to the post             | Fake URLs generated using Faker (https://instagram.com/post/abc123)              |
| caption       | string    | The text caption accompanying the post | Short fake sentence or phrase using Faker                                        |
| reach         | int       | Number of people who saw the post      | Random int between 1,000 and 50,000 — loosely correlates with follower count     |
| likes         | int       | Number of likes on the post            | Random int between 100 and 20,000 — loosely proportional to reach                |
| comments      | int       | Number of comments on the post         | Random int between 10 and 1,000 — lower than likes to simulate engagement ratios |


3. tracking_data - This dataset captures attribution data from users influenced by a campaign — essentially tying user actions back to an influencer and product.

   Number of rows - 15000
   

| Column        | Data type | Description                                                              | Assumptions Made                                                                                      |
|---------------|-----------|--------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------|
| source        | string    | The traffic source or referrer  domain that led the user to the purchase | Simulated as a random domain  (e.g., fitshop.net) to reflect UTMs,  tracking URLs, or affiliate links |                                                                       
| campaign      | string    | Name or ID of the marketing campaign                                     | Format: Campaign_# where # ranges from 1 to 10                                                        |
| influencer_id | int       | Reference to the influencer  (influencers.id) who drove the traffic      | Randomly selected to simulate multi-influencer campaigns                                              |
| user_id       | string    | Unique identifier of the  user who placed an order                       | Format: user_# with random  values simulating unique consumers                                        |
| product       | string    | Product purchased by the user                                            | Chosen from a realistic product list  from brands like MuscleBlaze, etc.                              |
| brand         | string    | The brand to which the product belongs                                   | Derived automatically from the  product using a mapping dictionary                                    |
| date          | date      | Date of the conversion  (i.e., when user placed the order)               | Random date in the last 60 days (-60d to today)                                                       |
| orders        | int       | Number of units ordered by the user                                      | Integer between 1 and 9, assuming small  but multiple quantities per order                            |
| revenue       | float     | Total revenue generated  from the user order                             | Random float between ₹20 and ₹500,  simulating average cart value in health and fitness e-commerce    |  

4. payouts - This dataset contains how much each influencer is paid, based either on per post or per order.

   Number of rows - 500
   
| Column        | Data type | Description                                                                                  | Assumptions Made                                                  |
|---------------|-----------|----------------------------------------------------------------------------------------------|-------------------------------------------------------------------|
| influencer_id | int       | Reference to the influencer (influencers.ID)                                                 | Matches the influencer IDs in the influencers table               |
| basis         | string    | The payout model used: either post (flat rate per post) or order (performance-based)         | Randomly chosen between "post" and "order"                        |
| rate          | float     | The amount paid per post or per order, depending on the basis                                | Random float between ₹5 and ₹50                                   |
| orders        | int       | If basis = order, this indicates the number of attributed orders; if post, it's just a dummy | Simulated between 5 and 100 for realism; unused if basis = "post" |
| total_payout  | float     | The total amount paid to the influencer: rate * orders (for order basis) or just rate (post) | Calculated accordingly                                            |

2. Tableau Dashboard
   - Uploaded influencer campaign data and modeled data as follows
     - influencer(id) 1--M posts(influencer_id)
     - influencer(id) 1--M tracking_data(influencer_id)
     - influencer(id) 1--1 payouts(influencer_id)

    ![data model](https://github.com/Ittismita/HealthKart_TrackingInfluencerCampaigns/blob/main/img/data_model.png)

  - Created Calculated fields:
    1. ERR - This is the most common engagement rate formula. ERR measures the percentage of people who interact with your content after seeing it.
           - ERR = total number of engagements on a post(likes+comments) / reach of that post * 100
    2. Incremental ROAS(Return on Ad Spend) - A metric that measures the additional revenue generated from an advertising campaign above and beyond what would have been earned without the campaign. Example: Let's say a brand spends $1000 on a campaign and sees $5000 in total revenue. The standard ROAS is 5:1. However, through incrementality testing, it's determined that $2000 of that revenue would have happened anyway (without the ad). iROAS would then calculate (5000-2000)/1000 = 3:1, indicating that the true incremental revenue from the campaign is $3 for every $1 spent. 

       For this supporting calculated fields were created like
       - Baseline Revenue - Assuming a 30% lift in revenue due to influencer impact = Revenue/1.3
       - Incremental Revenue = Revenue - Baseline Revenue
       - Finally, Incremental ROAS = Incremental Revenue / Total Payout
     
- Track performance of posts and influencers
- ROI and incremental ROAS calculation
- Filtering by brand, product, influencer type, platform
- Insights like: top influencers, best personas, poor ROIs
- Optional: export to CSV/PDF
