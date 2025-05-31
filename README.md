# AWS-Portfolio-cicd
This repository contains the CI/CD pipeline for deploying a static AWS portfolio website using Amazon S3 and GitHub Actions.

# Architecture Diagram

![image](https://github.com/user-attachments/assets/4c26a75b-e11f-4047-a414-f658c349467d)

🚀 Tech Stack
HTML/CSS/JS – Portfolio Website

AWS S3 – Static Website Hosting

IAM – Secure Role-based Access

GitHub – Source Code Hosting

GitHub Actions – CI/CD Pipeline

🧱 Architecture Overview
Developer pushes changes to GitHub.

GitHub Actions triggers a CI workflow.

The workflow deploys the latest code to an S3 bucket using IAM credentials.

The site is instantly available live via AWS S3.

📁 Folder Structure
bash
Copy
Edit
root/
├── website/                # Portfolio source files
│   ├── index.html
│   ├── style.css
│   └── ...
└── .github/
    └── workflows/
        └── deploy.yml      # GitHub Actions workflow
🛠️ Setup Instructions
1. Clone and Prepare Repository
bash
Copy
Edit
git clone https://github.com/your-username/aws-portfolio-cicd.git
cd aws-portfolio-cicd
Place your portfolio code inside the website/ directory.

2. Create and Configure S3 Bucket
Go to AWS Console → S3 → Create Bucket

Enable Static Website Hosting under Properties

Set:

Index document: index.html

Make the bucket public using a bucket policy:

json
Copy
Edit
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::your-bucket-name/*"
    }
  ]
}
3. Set Up IAM Role
Create an IAM user with programmatic access

Attach this inline policy:

json
Copy
Edit
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "s3:*",
      "Resource": [
        "arn:aws:s3:::your-bucket-name",
        "arn:aws:s3:::your-bucket-name/*"
      ]
    }
  ]
}
4. Add GitHub Secrets
Go to your repo → Settings → Secrets → Actions and add:

Secret Name	Value
AWS_ACCESS_KEY_ID	From IAM user
AWS_SECRET_ACCESS_KEY	From IAM user
AWS_REGION	e.g., us-east-1
AWS_S3_BUCKET	Your bucket name

5. Configure GitHub Actions Workflow
Create .github/workflows/deploy.yml:

yaml
Copy
Edit
name: Deploy to S3

on:
  push:
    branches: [main]

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout Code
      uses: actions/checkout@v4

    - name: Sync files to S3
      uses: jakejarvis/s3-sync-action@master
      with:
        args: --delete
      env:
        AWS_S3_BUCKET: ${{ secrets.AWS_S3_BUCKET }}
        AWS_ACCESS_KEY_ID: ${{ secrets.AWS_ACCESS_KEY_ID }}
        AWS_SECRET_ACCESS_KEY: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
        AWS_REGION: ${{ secrets.AWS_REGION }}
        SOURCE_DIR: "website"
6. Deploy 🚀
Push changes to the main branch:

bash
Copy
Edit
git add .
git commit -m "Deploy portfolio"
git push origin main
GitHub Actions will automatically deploy your site to S3.

🌐 Access Your Site
Visit:

arduino
Copy
Edit
http://your-bucket-name.s3-website-your-region.amazonaws.com
Example:

arduino
Copy
Edit
http://my-portfolio-site.s3-website-us-east-1.amazonaws.com
