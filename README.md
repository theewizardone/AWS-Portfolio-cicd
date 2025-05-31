# AWS-Portfolio-cicd
This repository contains the CI/CD pipeline for deploying a static AWS portfolio website using Amazon S3 and GitHub Actions.

# Architecture Diagram

![image](https://github.com/user-attachments/assets/4c26a75b-e11f-4047-a414-f658c349467d)

🚧 Prerequisites:
AWS Account

S3 Bucket (for hosting your portfolio)

IAM User or Role with S3 access

GitHub Repository with your portfolio code (HTML/CSS/JS)

GitHub Actions enabled

🔧 Step-by-Step Instructions:
Step 1: Create and Configure S3 Bucket
Go to AWS Console → S3 → Create bucket

Bucket settings:

Name it uniquely (e.g., my-portfolio-site)

Enable Static Website Hosting in the "Properties" tab

Set index document to index.html

Set permissions:

Make bucket public (if needed) by editing the bucket policy:

json

{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PublicReadGetObject",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::my-portfolio-site/*"
    }
  ]
}
Step 2: Set Up IAM Credentials
Go to IAM → Users → Add user

Add programmatic access (access key ID and secret)

Attach policy:

json

{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": ["s3:*"],
      "Resource": ["arn:aws:s3:::my-portfolio-site", "arn:aws:s3:::my-portfolio-site/*"]
    }
  ]
}
Save AWS_ACCESS_KEY_ID and AWS_SECRET_ACCESS_KEY.

Step 3: Create GitHub Repo
Create a new repository (e.g., AWS-Portfolio-cicd)

Push your portfolio files to it (HTML, CSS, JS, images)

Step 4: Add Secrets to GitHub
Go to Settings → Secrets → Actions and add:

Name	Value
AWS_ACCESS_KEY_ID	Your AWS access key
AWS_SECRET_ACCESS_KEY	Your AWS secret key
AWS_REGION	Your bucket's region (e.g., us-east-1)
AWS_S3_BUCKET	Name of the bucket (e.g., my-portfolio-site)

Step 5: Set Up GitHub Actions Workflow
Create .github/workflows/deploy.yml:

yaml

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
Make sure your portfolio files are in a website/ directory.

Step 6: Push Changes and Deploy
Push changes to the main branch

GitHub Actions will:

Run the workflow

Deploy your site to the S3 bucket

✅ Bonus: Access Your Portfolio
Visit:

php-template
http://<your-bucket-name>.s3-website-<region>.amazonaws.com
