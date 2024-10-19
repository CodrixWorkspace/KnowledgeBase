# Knowledge Base

* Repo to store Knowledge Articles
* A GIT Action pipeline will sync the articles pushed into this repo into S3 bucket


## Stage Environment

### Sync Bucket Content 

Run `aws s3 sync articles/ s3://skillhunt-knowledge-base/articles/` to sync bucket content


### List Bucket Content 

Run `aws s3 ls s3://skillhunt-knowledge-base` to view bucket content
