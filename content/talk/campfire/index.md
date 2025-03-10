---
title: "WhatsApp Chat Analysis and Deployment with Heroku"
subtitle: "Analyzing Personal WhatsApp Communication Patterns Through Data Visualization and Web Deployment "
excerpt: "This project presents an analysis of WhatsApp chat data, aiming to extract meaningful insights into messaging behaviors and patterns. By processing exported chat logs, the application identifies peak messaging times, active users, and prevalent content themes. Developed using Python's data analysis libraries and deployed on Heroku, it offers a scalable and accessible solution for users seeking to understand and visualize their WhatsApp communication trends. "
date: 2024-12-01
date_end: "2024-12-20"
featured: true
show_post_time: false
author: "Shivam Chhetry"
location: "Punjab, India"
draft: false
# layout options: single, single-sidebar
layout: single
categories:
- NLP
- Python
links:
- icon: door-open
  icon_pack: fas
  name: website
  url: https://whatsappchatanalysis.herokuapp.com/
- icon: github
  icon_pack: fab
  name: code
  url: https://github.com/shivamkc01/WhatsAppChat_AnalysisDeployusingHeroku
---

## Project Overview

WhatsApp serves as a primary communication tool for millions worldwide, generating extensive chat data. Analyzing this data can reveal patterns, peak activity times, and user engagement levels. This project focuses on processing exported WhatsApp chat logs to extract valuable insights, including:

- **Message Frequency Analysis**: Identifying peak messaging times and active days.
- **User Activity Assessment**: Determining the most active users and their contribution levels.
- **Content Analysis**: Highlighting frequently used words, emojis, and media types.
- **Temporal Patterns**: Analyzing message distribution over days, months, and years.

By processing the exported chat data, the project offers a deeper understanding of communication dynamics within WhatsApp groups or individual conversations.

## Key Features

- **Data Preprocessing**: Utilizes Python's `re` and `pandas` libraries to clean and structure raw chat data into a DataFrame suitable for analysis.
- **Analytical Insights**: Employs `matplotlib` and `seaborn` for visualizations, presenting:
  - Overall message frequency.
  - Top active users and their messaging statistics.
  - Commonly used words and emojis.
  - Heatmaps indicating activity across different times and dates.
- **Web Application Deployment**: Integrates the analysis with a user-friendly web interface using Streamlit. The application is deployed on Heroku, making it accessible online.

## Getting Started

To replicate or extend this project, follow these steps:

1. **Export WhatsApp Chat**:
   - Open the desired chat in WhatsApp.
   - Tap on the contact or group name to access chat information.
   - Select "Export chat" and choose "Without Media" to obtain a text file of the conversation.

2. **Set Up the Environment**:
   - Clone the repository:
     ```bash
     git clone https://github.com/shivamkc01/WhatsAppChat_AnalysisDeployusingHeroku.git
     ```
   - Navigate to the project directory:
     ```bash
     cd WhatsAppChat_AnalysisDeployusingHeroku
     ```
   - Install required dependencies:
     ```bash
     pip install -r requirements.txt
     ```

3. **Data Analysis**:
   - Place the exported chat text file in the project directory.
   - Run the analysis script to process the chat data and generate visualizations.

4. **Web Application**:
   - Launch the Streamlit app locally:
     ```bash
     streamlit run app.py
     ```
   - Access the app at `http://localhost:8501` to interact with the analysis.

5. **Deployment on Heroku**:
   - Ensure you have a Heroku account and the Heroku CLI installed.
   - Create a new Heroku app and deploy the project using the provided `Procfile` and `setup.sh`.
   - Once deployed, access the live application via the provided Heroku URL.

## Technologies Used

- **Programming Language**: Python
- **Data Analysis**:
  - `pandas`
  - `re`
- **Visualization**:
  - `matplotlib`
  - `seaborn`
- **Web Framework**:
  - Streamlit
- **Deployment**:
  - Heroku

## Try It Out
