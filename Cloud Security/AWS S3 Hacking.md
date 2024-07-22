S3 Bucket allows us to store things in containers called **buckets**.
The files stored in the Amazon S3 bucket are called **S3 objects**.

```sh
# Installation
apt install awscli -y
```
```sh
# Configuration
aws configure
```

---
# Private S3
##### List all S3 Buckets
```sh
aws --endpoint=http://[domain_name] s3 ls

# Example
aws --endpoint=http://s3.thetoppers.htb s3 ls
```

![[Pasted image 20231103091258.png]]


#### List all objects under the bucket
```sh
aws --endpoint=http://s3.thetoppers.htb s3 ls s3://[bucket_name]
```

![[Pasted image 20231103091417.png]]

##### File Operations
```sh
# File Upload
aws --endpoint=http://s3.thetoppers.htb s3 cp shell.php s3://thetoppers.htb

# File Download
aws --endpoint=http://s3.thetoppers.htb/[file_to_download] cp s3://thetoppers.htb [file_name_to_save_with]
```

---
# Public S3
For *some* public S3 buckets, you don't need to configure before-hand. We can use the `--no-sign-request`

##### List objects under the bucket
```sh
aws s3 ls s3://[bucket_name] --no-sign-request
```

##### File Operations
```sh
# Download
aws cp s3://thetoppers.htb/[filename] [file_name_to_save_with] --no-sign-request

# Upload
aws s3 cp shell.php s3://thetoppers.htb --no-sign-request
```
