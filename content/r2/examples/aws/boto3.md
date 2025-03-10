---
title: boto3
pcx_content_type: configuration
---
# Configure `boto3` for R2

{{<render file="_keys.md">}}<br>

You must configure [`boto3`](https://boto3.amazonaws.com/v1/documentation/api/latest/index.html) to use a preconstructed `endpoint_url` value. This can be done through any `boto3` usage that accepts connection arguments; for example:

```python
import boto3

s3 = boto3.resource('s3',
  endpoint_url = 'https://<accountid>.r2.cloudflarestorage.com',
  aws_access_key_id = '<access_key_id>',
  aws_secret_access_key = '<access_key_secret>'
)
```

You may, however, omit the `aws_access_key_id` and `aws_secret_access_key ` arguments and allow `boto3` to rely on the `AWS_ACCESS_KEY_ID` and `AWS_SECRET_ACCESS_KEY` [environment variables](https://boto3.amazonaws.com/v1/documentation/api/latest/guide/configuration.html#using-environment-variables) instead.

An example script may look like the following:

```python
---
filename: main.py
---
import boto3

s3 = boto3.client(
    service_name ="s3",
    endpoint_url = 'https://<accountid>.r2.cloudflarestorage.com',
    aws_access_key_id = '<access_key_id>',
    aws_secret_access_key = '<access_key_secret>'
    region_name="<location>", # Must be one of: wnam, enam, weur, eeur, apac, auto
)

# Get object information
object_information = s3.head_object(Bucket=<R2_BUCKET_NAME>, Key=<FILE_KEY_NAME>)

# Upload/Update single file
s3.upload_fileobj(io.BytesIO(file_content), <R2_BUCKET_NAME>, <FILE_KEY_NAME>)

# Delete object
s3.delete_object(Bucket=<R2_BUCKET_NAME>, Key=<FILE_KEY_NAME>)
```

```sh
$ python main.py
# Buckets:
#  -  user-uploads
#  -  my-bucket-name
# Objects:
#  -  cat.png
#  -  todos.txt
```
