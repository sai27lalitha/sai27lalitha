from googleapiclient.discovery import build

from pprint import pprint

import streamlit as st

api_key = "AIzaSyB5AZbx_ADOBc9tJ5Tin8_jLzLbgyPwk3o"

youtube = build("youtube", "v3", developerKey=api_key)

def channel_details_scrape(user_input):
  request = youtube.channels().list(part="snippet,contentDetails,statistics", id=user_input)
  response = request.execute()
  chanel_scrape_details=dict(chanel_title=response['items'][0]['snippet']['title'], 
                            id=response['items'][0]['id'], 
                            description=response['items'][0]['snippet']['description'],
                            published_date=response['items'][0]['snippet']['publishedAt'],
                            subscribers=response['items'][0]['statistics']['subscriberCount'],
                            videos=response['items'][0]['statistics']['videoCount'],
                            views=response['items'][0]['statistics']['viewCount'])
  return chanel_scrape_details
  
id=st.text_input('Channel ID', 'Enter the channel id you want scrape')

dEtails=channel_details_scrape(id)

st.write(dEtails)
