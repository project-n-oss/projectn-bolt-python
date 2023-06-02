# Granica SDK

This SDK provides an authentication solution for programatically interacting with Granica. It wraps the boto3 interface so project wide integration is as easy as refactoring `import boto3` to `import granica as boto3`.

The package affects the signing and routing protocol of the boto3 S3 client, therefore any non S3 clients created through this SDK will be un-affected by the wrapper.

## Prerequisites

The minimum supported version of Python is version 3.

## Installation

```bash
python3 -m pip install granica-sdk
```

## Configuration

For the client to work it must have knowledge of Granica's *service discovery url*.
These are parameterized by the *region* of Granica's deployment. A preferred *availability zone ID* can also be provided for AZ-aware routing.

**Configure the Granica custom domain:**
Declare the ENV variable: `GRANICA_CUSTOM_DOMAIN`, which constructs Granica URL and hostname based on default naming, and AWS region.
```bash
export GRANICA_CUSTOM_DOMAIN="example.com"
```

**There are two ways to expose Granica's region/preferred availability zone to the SDK:**

1. If running on an EC2 instance the SDK will by default use that instance's region and zone ID
2. With the ENV variables: `AWS_REGION` and `AWS_ZONE_ID`.
```bash
export AWS_REGION='<region>'
export AWS_ZONE_ID='<az-id>'
```

## Debugging

Import the default logger and set its level to DEBUG

`logging.getLogger().setLevel(logging.DEBUG)`

## Example S3 GetObject

```python
import granica as boto3

s3_client = boto3.client('s3')
response = s3_client.get_object(Bucket='MyBucket', Key='MyKey.csv')
obj = response['Body'].read()

```

## Tests
Basic integration tests are provided for the modified Session/Client interfaces. They must be run in an environment with a properly configured Granica deployment accessible.
```bash
python3 tests/tests.py
```
