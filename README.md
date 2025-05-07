��� Deployment Documentation – MERN Stack Blog App on AWS
This document explains the steps I followed to successfully deploy a MERN stack blog application on AWS using EC2 and S3, with MongoDB Atlas for data storage.

✅ Step-by-Step Deployment Summary
1. MongoDB Atlas Setup
Created a MongoDB Atlas account and launched a free-tier cluster.

Added my EC2 instance’s IP to the access list.

Created a database user and copied the connection string into the backend .env file.

2. S3 Bucket for Frontend
Created an S3 bucket named tareq-blogapp-frontend in the eu-north-1 region.

Disabled “Block all public access” for the bucket.

Enabled static website hosting with index.html as the default document.

Added a public-read bucket policy to allow access to the hosted React app.

After building the frontend, uploaded it using:

bash
نسخ
تحرير
aws s3 sync dist/ s3://tareq-blogapp-frontend/
3. S3 Bucket for Media Uploads
Created a second bucket named tareq-blogapp-backend.

Disabled “Block all public access”.

Added a CORS configuration to allow file uploads from the browser.

Tested file uploads successfully from the frontend and verified media was accessible.

4. IAM User and Policy
Created IAM user blog-app-user.

Created and attached a custom IAM policy granting s3:PutObject, s3:GetObject, s3:DeleteObject, and s3:ListBucket access to both buckets.

Used the Access Key ID and Secret Access Key to configure AWS CLI on the EC2 instance and saved them securely in the backend .env.

5. EC2 Instance Setup (Backend)
Launched an Ubuntu 22.04 EC2 instance (t3.micro) in the eu-north-1 region.

Updated inbound rules to allow ports 22, 80, 443, and 5000.

SSH’d into the instance and ran the user data script to install:

Node.js via NVM

PM2 for process management

MongoDB Shell

AWS CLI

Cloned the project repo and set up the backend environment variables including:

MongoDB URI

AWS credentials

S3 bucket: tareq-blogapp-backend

Installed dependencies and used PM2 to run the backend.

6. Frontend Configuration and Deployment
Set up the frontend .env file with:

env
نسخ
تحرير
VITE_BASE_URL=http://<ec2-public-dns>:5000/api
VITE_MEDIA_BASE_URL=https://tareq-blogapp-backend.eu-north-1.amazonaws.com
Installed frontend dependencies using pnpm.

Built the app with pnpm run build.

Synced the final build to the tareq-blogapp-frontend bucket.

��� Testing & Verification
Confirmed backend was running using pm2 list.

Loaded the frontend via S3 public URL.

Created posts, uploaded media, and verified entries in MongoDB.

Validated that media files were stored in tareq-blogapp-backend.
