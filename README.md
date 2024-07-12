<div align="center" style="border-radius: 15px">
    <img src="logo.png" height = "200" style="border-radius: 15px;"/>
    <p style="font-size:1.2rem;"><a target="_blank" href="https://jobsdreamer.com">JobsDreamer</a></p>
    <p style="font-size:1rem;">All internships on the internet in last 24 hours to your email</p>
</div>

## Technical Report

- The software collects scraped data from the internet and processes it to provide a list of internships to the user. The code is closed source.

### Overview

<div align="center" style="border-radius: 5px">
    <img src="overview.png" width="100%" style="border-radius: 5px;"/>
</div>

- The software is divided into two main parts: Web Scraping and Data Processing Pipeline.
- **Pipeline:**
- 1. Schedule the Web Scraping process to run every 24 hours using AWS EventBridge.
- 2. All processes are orchestrated using AWS Step Functions to automate workflows by connecting various AWS services and custom application logic.
- 3. Create a NAT Gateway using AWS Lambda to allow the VPC to access the internet. Creating and deleting the NAT Gateway as needed can help save 80% of the costs compared to keeping the NAT Gateway running 24/7 when it's not in use.
- 4. Preprocess new search terms and group similar search terms for web scraping, instead of using all the search terms, and save 90% of the time and cost.
- 5. [Web Scraping:](#web-scraping) The software scrapes the internet for internships using the search terms and saves the data to an S3 bucket.
- 6. [Data Processing Pipeline:](#data-processing-pipeline) The software processes the scraped data to provide a list of internships to the user.
- 7. Sending an email to the user with the list of internships with their desired position only using AWS SES.
- 8. Delete the NAT Gateway after all processes are completed to save costs.

### Web Scraping

<div align="center" style="border-radius: 5px">
    <img src="web-scraping.png" width="100%" style="border-radius: 5px;"/>
</div>

- 5.1 - The software uses a list of search terms to scrape the internet for internships from various job sites.
- 5.2 - The container is pulled from the Amazon Elastic Container Registry (ECR) and run on AWS Fargate.
- 5.3 - The software go to the internet through a proxy server.

### Data Processing Pipeline

<div align="center" style="border-radius: 5px">
    <img src="data-pipeline.png" width="100%" style="border-radius: 5px;"/>
</div>

- 6.1 - Download the raw data from AWS S3 bucket.
- 6.2 - **Cleaning**: delete duplicate data and clean the data.
- 6.3 - **Categorizing and Reviewing**: categorize the each internship by the search term with fine-tuned Llama 3 and final review by GPT 4o.

### Technologies

- **Python, Scrapy, Selenium:** used for web scraping.
- **AWS Fargate, AWS Lambda, AWS Elastic Container Registry (ECR)**: used for containerization of the web scraping process.
- **AWS Step Functions, AWS EventBridge, AWS Simple Email Service (SES), AWS S3**: used for orchestrating the workflow and sending emails.
- **Fine-tuned Llama 3, GPT 4o**: used for categorizing and reviewing the internships.
- **AWS Virtual Private Cloud (VPC), AWS NAT Gateway, AWS Elastic IP, AWS Internet Gateway**: used for connecting the VPC to the internet.
- **ISP Proxy Server**: used for connecting to the internet and avoiding IP blocking.
- **GitHub Actions**: used for CI/CD.
