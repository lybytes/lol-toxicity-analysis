# Overview

This project was completed as part of the ST445: Managing and Visualising Data course at the London School of Economics and Political Science (LSE). It was submitted as a group project in the 2025/27 academic year.

# Problem Statement

Online toxicity is a pervasive issue in competitive gaming. League of Legends, one of the world's most popular multiplayer online battle arena (MOBA) games with over 150 million registered players, has long struggled with toxic player behaviour — including harassment, hate speech, and deliberate sabotage of teammates. Understanding the patterns behind this behaviour is valuable both for game developers seeking to improve player experience and for broader research into online community dynamics.
This project investigates toxicity in League of Legends by analysing player behaviour data retrieved via the Riot Games API, combining it with natural language processing techniques to detect and understand patterns of toxic communication across different player roles, ranks, and game outcomes.

# Key Features

Data collection via the Riot Games API
Champion and player position analysis
Toxicity classification using NLP models from HuggingFace
Interactive visualisations including Sankey diagrams and choropleth maps
Clustering analysis to identify behavioural archetypes

# Requirements
Before running this project, you will need to obtain two API credentials:
1. Riot Games API Key
Register at the Riot Developer Portal
Generate an API key and paste it into ST445_Project_Riot API Key.txt

2. HuggingFace Token
Create an account at HuggingFace
Generate an access token under Settings → Access Tokens
Paste it into ST445_Project_hf token.txt

# Data
The project uses a combination of data sources:

Riot API — live player and match data
OP.GG — champion position data
Pre-processed datasets included in the repository (final_cleaned_data_no_labels.csv, section3_*.csv)

# Disclaimer
This project is purely academic. All data was collected in accordance with the Riot Games API Terms of Service. No personal player data is stored or distributed.
