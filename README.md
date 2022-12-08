# Health_Influencer_Credibility
Data of desk research and instagram analyses attached 

Python Code for the Instagram analysis of health influencer profile and followers' engagement

```#!/usr/bin/env python
# coding: utf-8

import instaloader
from datetime import datetime
from itertools import dropwhile, takewhile
import pandas as pd
import csv

L = instaloader.Instaloader()
L.login("INSTAGRAM-USER","USER-PASSWORD")


# Read CSV File with instagram profiles
influencer = pd.read_csv("influencer.csv", header=0)


SINCE = datetime(2021, 1, 1)
UNTIL = datetime(2022, 3, 1)
data = []


# Iterate through csv file and retrieve all posts in the defined TimeFrame
for index, row in influencer.iterrows():
    print("Loading Influencer: ", row.Name)
    profile = instaloader.Profile.from_username(L.context, row.Name)
    followers = profile.followers
    for post in profile.get_posts():
        if(post.date_utc < UNTIL and post.date_utc > SINCE):
            print("Post vom:", post.date_utc)
    		# Append Retrieved Data
            data.append(
            {
                'URL': post.url,
                'Influencer': post.owner_username,
                'Follower' : followers,
                'Datum': post.date_utc,
                'Likes': post.likes,
                'Kommentare': post.comments,
                'Beschreibung': post.caption,
                'Posttyp': post.typename
            }
            )

df = pd.DataFrame(data)

df.to_csv("influencerposts.csv", sep=";", header=True, index=False)```
