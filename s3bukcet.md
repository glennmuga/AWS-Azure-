
## First create bucket on aws 
Open Aws , search S3 Bucket >> Create S3 bucket 

Choose Create bucket.

Enter the Bucket name (for example, example.com).

Choose the Region where you want to create the bucket.

Uncheck the check box for block all public access

Click on the acknowledge checkbox

## Enable static web hosting 

In the Buckets list, choose the name of the bucket that you want to enable static website hosting for.

Choose Properties.

Under Static website hosting, choose Edit.

Choose Use this bucket to host a website.

Under Static website hosting, choose Enable.

In Index document, enter the file name of the index document, typically index.html.

Choose Save changes.

Amazon S3 enables static website hosting for your bucket. At the bottom of the page, under Static website hosting, you see the website endpoint for your bucket.

Under Static website hosting, note the Endpoint.

The Endpoint is the Amazon S3 website endpoint for your bucket. After you finish configuring your bucket as a static website, you can use this endpoint to test your website.

## Add a bucket policy that makes your bucket content publicly available

Under Buckets, choose the name of your bucket.

Choose Permissions.

Under Bucket Policy, choose Edit.

To grant public read access for your website, copy the following bucket policy, and paste it in the Bucket policy editor.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "PublicReadGetObject",
            "Effect": "Allow",
            "Principal": "*",
            "Action": [
                "s3:GetObject"
            ],
            "Resource": [
                "arn:aws:s3:::Bucket-Name/*"
            ]
        }
    ]
}

```

## Create a new repository in your GitHub account
 >> add index.html and paste the following content

<html xmlns="http://www.w3.org/1999/xhtml" >
<head>
    <title>My Website Home Page</title>
</head>
<body>
  <h1>Welcome to my website</h1>
  <p>Now hosted on Amazon S3!</p>
</body>
</html>

## On your newly created repository, click on settings
Select Secrets and Variables from settings, and select Actions

 Select New repository secret.

 Enter AWS_ACCESS_KEY_ID in the Name field, enter your AWS access key in the Value field, and click Add secret.

 Select New repository secret again.

Enter AWS_SECRET_ACCESS_KEY in the Name field, enter your AWS secret key in the Value field, and click Add secret.

Create a folder and name it .github (please make sure the .(dot) is part of your .github name), 
inside the .github folder create a new folder and call it workflows, create a file called main.yml inside .github/workflows folder

you should have -> .github/workflows/main.yml

Enter the following code in your main.yml

``` yml
name: Upload Website

on:
  push:
    branches:
    - main

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
    - name: Checkout
      uses: actions/checkout@v1

    - name: Configure AWS Credentials
      uses: aws-actions/configure-aws-credentials@v1
      with:
        aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
        aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
        aws-region: us-east-1

    - name: Deploy static site to S3 bucket
      run: aws s3 sync . s3://BUCKET_NAME --delete

```

Modified BUCKET_NAME with your AWS S3 bucket name, along with the aws_region.

## go to actions tab

In the Buckets list, choose the name of the bucket that you enable static website hosting.

Choose Properties.

Under Static website hosting, click on the Endpoint URL.

