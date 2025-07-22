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
1. influencers
   - 500 unique influencers
   - id - unique id of the influencer
   - name - Name of the influencer
   - category - The 4 categories - Fitness, Health, Lifestyle, Parenting
   - 3 Gender categories - Male, Female, Other
   - Follower count - random number between 10000 to 1000000
   - 4 Platforms - Instagram, Twitter, Tiktok, Youtube

3. posts
   - 10000 rows
   - URL - URL of the post.
   - date - ranging between April 23, 2025 and July 22, 2025 - 3 months
   - caption - random one - liners
   - reach - The number of unique users who viewed or were exposed to a given post. A random number between 1000 and 50000
   - likes - Number of likes on respective posts
   - comments - Number of comments on respective posts

4. tracking_data
   - source - The traffic source or referrer domain that led the user to the purchase
   - campaign	- Name or ID of the marketing campaign - Format: Campaign_# where # ranges from 1 to 10
   - influencer_id - Reference to the influencer (influencers.id) who drove the traffic
   - user_id - Unique identifier of the user who placed an order - Format: user_# with random values simulating unique consumers
   - product - Product purchased by the user - Chosen from a realistic product list from brands like MuscleBlaze, HKVitals, Gritzo
   - brand - The brand to which the product belongs
   - date -	Date of the conversion (i.e., when user placed the order) -	Random date in the last 60 days (-60d to today)
   - orders -	Number of units ordered by the user -	Integer between 1 and 9, assuming small but multiple quantities per order
   - revenue - Total revenue generated from the user order - Random float between ₹20 and ₹500, simulating average cart value in health and fitness e-commerce
