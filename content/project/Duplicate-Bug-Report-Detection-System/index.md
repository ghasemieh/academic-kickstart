---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "Duplicate Bug Report Detection System"
summary: ""
authors: [Alireza]
tags: [NLP, Bug Report]
categories: [NLP]
date: 2020-02-19T09:41:51-05:00

# Optional external URL for project (replaces project detail page).
external_link: ""

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
# Focal points: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight.
image:
  caption: ""
  focal_point: "Smart"
  preview_only: true

# Custom links (optional).
#   Uncomment and edit lines below to show custom links.
links:
 - name: Github
   url: https://github.com/ghasemieh/Duplicated-Bug-Report-Detection-System
   icon_pack: fab
   icon: github

url_code: ""
url_pdf: ""
url_slides: ""
url_video: ""

# Slides (optional).
#   Associate this project with Markdown slides.
#   Simply enter your slide deck's filename without extension.
#   E.g. `slides = "example-slides"` references `content/slides/example-slides.md`.
#   Otherwise, set `slides = ""`.
slides: ""
---
![Image of Bug](/img/BR1.png)

## Background
As software programs become increasingly large and complex, it is important to improve the quality of software maintenance. Bug report recommendations can significantly improve the triaging of bug reports. It is difficult to inspect the new incoming reports manually to route to the developers who have fixed the duplicate bugs. Automatic identification of Duplicate bug reports is a critical research problem in the software repositories’ mining area.

## Aim
The project aims to propose an effective unsupervised and supervised models to detect duplicate bug report in the Bugzilla repository. The search engine finds the top-N most similar reports to a given report, and deduplicate issues faster. Moreover, it presents an analytical dashboard to developers to understand the different aspects of the bug reports’ statistics and major sources of bug generation.

## ETL Process
The search engine extracts data using the Bugzilla REST API https://wiki.mozilla.org/Bugzilla:REST_API and creates a data-lake using MongoDB. Then it verifies the data quality and conducts data wrangling and cleaning. 

## Data Preparation
Since the data is considered as big data the engine loads the data to Hadoop HDFS and performs text preprocessing using PySpark which includes: 
- Converting text to lowercase
- Splitting the words into 3 steps using 
  1. ASCII character identification for English 
  2. split by space  
  3. Wordninja
- Applying normalizes
- Applying contractions or expansions
- Removing punctuations, tags, special characters, digits
- Stemming
- Lemmatization. 

Then it stores the processed data in PostgreSQL. 

## Evaluation
The empirical evaluation is performed on the open datasets of the Bugzilla repository. The metrics used for evaluation are Mean Average Precision (MAP), Mean Reciprocal Rank (MRR) and Recall rate. 

## Visualization and Presentation
The top-N most similar reports to a given report are presented on a web page using Flask. Also, It presents the developer the statistical information about the bug reports in a dashboard using D3.js

## Implementation Method
Implementation Method: The search engine is implemented on AWS using Docker Composer and ECS with Fargate

![Image of Application Process](/img/ApplicationStructure.jpg)

# Installation Guide

## System Requirement
You need to have at least 10GB of FREE RAM available to run this application.

To run the application there are two options:
- Use the docker image
- Use the source code 

## Implement the Application Using Docker

Please follow the steps below:

1. Install docker CE from https://docs.docker.com/install/linux/docker-ce/ubuntu/

2. Install docker compose from https://docs.docker.com/compose/install/

3. Create a vim file with name `docker-compose.yml` where you want to run the application using the content below:

```
version: '3'
services:
  web:
    build: .
    image: applia65/duplicatebugreportsearchengine:web
    # restart: always
    environment:
      DATABASE_HOST: postgres_docker
      DATABASE_USER: postgres
      DATABASE_PASSWD: password123
      DATABASE_DATABSE_NAME: bug_database
      MONGO_ADDRESS: mongodb://mongodb_docker:27017/
    ports:
      - "0.0.0.0:5000:5000"
    depends_on:
      - postgres_docker
      - mongodb_docker

  postgres_docker:
    image: postgres:10
    # restart: always
    # # Allow access from Development machine
    # ports:
    #  - "0.0.0.0:5432:5432"
    volumes:
      - ./pg_data/:/var/lib/postgresql/data:Z
    environment:
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: password123
      POSTGRES_DB: bug_database

  mongodb_docker:
    image: mongo:latest
    #restart: always
    #environment:
    #  MONGO_INITDB_ROOT_USERNAME: root
    #  MONGO_INITDB_ROOT_PASSWORD: example
```

4. Execute the code below where you create `docker-compose.yml` and wait until your server downloads all containers.

```
sudo docker-compose pull
```

5. Run the application.

```
sudo docker-compose up
```

6. Open your browser using `0.0.0.0:5000` address.

## Implement the Application Using the Source Code Ubuntu

Please follow the steps below:

1. Pull the codes by git

2. Install all requirements 

```
pip install --no-cache-dir -r requirements.txt
```

3. Install the WordNet which is about 900 MB

```
python -m spacy download en_core_web_lg
```

4. Install MongoDB from https://docs.mongodb.com/manual/tutorial/install-mongodb-on-ubuntu/

5. Run the MongoDB service

```
sudo systemctl start mongod
```

6. Verify that MongoDB has started successfully.

```
sudo systemctl status mongod
```

7. Install PostgreSQL Version 10 from https://www.postgresql.org/download/linux/debian/

8. Run the Postgresql service

```
sudo systemctl start postgresql
```

9. Verify that Postgresql has started successfully.

```
sudo systemctl status postgresql
```

10. Create a user `postgres` with password `password123`

11. Run the application 

```
python ./main.py
```

12. Open your browser using `0.0.0.0:5000` address.

### Please contact us if you have any question. 
#### - Alireza Ghasemieh `alireza@ghasemieh.com`
#### - Sukhjit Singh Sehra `sukhjit.sehra@ryerson.ca`

### Special thanks to **Sukhjit** for helping me with this project.

## References:
- POSTER: DWEN: DeepWord Embedding Network for Duplicate Bug Report Detection in Software Repositories, Amar Budhiraja, 2018 ACM/IEEE 40th International Conference on Software Engineering: Companion Proceedings
- Preventing duplicate bug reports by continuously querying bug reports, Abram Hindle, Empirical Software Engineering, https://doi.org/10.1007/s10664-018-9643-4
- A comparative study of the performance of IR models on duplicate bug detection, Nilam Kaushik, 2012 16th European Conference on Software Maintenance and Reengineering
- Duplicate Bug Report Detection with a Combination of Information Retrieval and Topic Modeling, Anh Tuan Nguyen, ASE ’12, September 3-7, 2012, Essen, Germany
- Studying the needed effort for identifying duplicate issues, Mohamed Sami Rakha, Empir Software Eng, DOI 10.1007/s10664-015-9404-6
- Revisiting the Performance Evaluation of Automated Approaches for the Retrieval of Duplicate Issue Reports, Mohamed Sami Rakha, DOI 10.1109/TSE.2017.2755005, IEEE Transactions on Software Engineering
- Detection of Duplicate Defect Reports Using Natural Language Processing, Per Runeson, 29th International Conference on Software Engineering (ICSE'07) 0-7695-2828-7/07
- Rediscovery Datasets: Connecting Duplicate Reports, Mefta Sadat, 2017 IEEE/ACM 14th International Conference on Mining Software Repositories (MSR)
- An Approach to Detecting Duplicate Bug Reports using Natural Language and Execution Information, Xiaoyin Wang, ICSE’08, May 10–18, 2008, Leipzig, Germany.
- Detecting Duplicate Bug Report Using Character N-Gram-Based Features, Ashish Sureka, 2010 Asia Pacific Software Engineering Conference
- Towards More Accurate Retrieval of Duplicate Bug Reports, Chengnian Sun
- A Discriminative Model Approach for Accurate Duplicate Bug Report Retrieval, Chengnian Sun, ICSE’10, May 2–8, 2010, Cape Town, South Africa
- Combining Word Embedding with Information Retrieval to Recommend Similar Bug Reports, Xinli Yang, 2016 IEEE 27th International Symposium on Software Reliability Engineering
