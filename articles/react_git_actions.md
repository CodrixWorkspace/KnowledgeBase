# Guide to Deploying a React Project to AWS S3 

This guide provides instructions that help you successfully deploy your React app on AWS S3 and serve it as a static website 

## Prerequisites

Before starting, ensure you have:

- AWS Account:  Set up an account on AWS if you haven't already.
- AWS CLI: Install the AWS Command Line Interface and configure it with your AWS credentials.

       `aws configure`

- React Project: A ready-to-deploy React project


## 1. Build the React Project
To prepare the project for production, build the React app. This generates static files optimized for deployment.

- Open your React project in the terminal.
  
- Run the following command to create a production-ready build:

       `npm run build`

- This will create a build directory with all the static assets for deployment.

## 2. Set Up an S3 Bucket

- Go to the S3 service on the AWS Console.
  
- Click on Create Bucket and provide a unique name (e.g., my-react-app).
  
- Select the AWS region closest to your users.
  
- Uncheck Block all public access to make your bucket publicly accessible (you’ll confirm this action later).
  
- Click Create bucket.

## 3. Configure the S3 Bucket for Static Website Hosting

- Navigate to the Properties tab of your newly created S3 bucket.
  
- Scroll down to Static website hosting and click Edit.

- Select Enable and specify the following:

   - Index document: index.html

   - Error document: index.html (to handle client-side routing)

- Save changes.

## 4. Upload the Build Files to the S3 Bucket

You can upload the files manually via the AWS Console or use the AWS CLI:

### Option A: Using AWS Console

- Go to your S3 bucket and open the Objects tab.
  
- Click Upload and select all the files in the build folder.
  
- Ensure all files have public read permissions.

### Option B: Using AWS CLI

- Run the following command in your terminal to upload the build files to S3:

      `aws s3 sync build/ s3://my-react-app --acl public-read`

- Replace my-react-app with your bucket name.

## 5. Set S3 Bucket Policy for Public Access

To make your files accessible on the web, set a bucket policy that allows public read access:

- In the S3 Console, go to the Permissions tab.

- Scroll down to Bucket Policy and click Edit.

- Paste the following JSON policy, replacing YOUR_BUCKET_NAME with your actual bucket name:
  
           {
              "Version": "2012-10-17",
              "Statement": [
                  {
                       "Effect": "Allow",
                       "Principal": "*",
                       "Action": "s3:GetObject",
                       "Resource": "arn:aws:s3:::YOUR_BUCKET_NAME/*"
                  }
               ]
           }

- Replace YOUR_BUCKET_NAME with your bucket name.


- Save changes.


## 6. Access the Static Website
- Go back to the Properties tab of your S3 bucket.
  
- In the Static website hosting section, you’ll see a URL endpoint.
  
- Click on the link to verify that your React app is live!


### This guide should help you successfully deploy your React app on AWS S3 and serve it as a static website. Let me know if you need more details on any step!











