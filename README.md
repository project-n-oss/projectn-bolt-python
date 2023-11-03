# Granica SDK

This SDK provides an authentication solution for programatically interacting with Granica. It wraps the boto3 interface so project wide integration is as easy as refactoring `import boto3` to `import granica as boto3`.

The package affects the signing and routing protocol of the boto3 S3 client, therefore any non S3 clients created through this SDK will be un-affected by the wrapper.

## Prerequisites

The minimum supported version of Python is version 3.

## Installation

```bash
python3 -m pip install granica-sdk
```

## Usage

In order to use this package, you need to set the following environment variables where your application will be running

```bash
export GRANICA_CUSTOM_DOMAIN=<YOUR_CUSTOM_DOMAIN>
export GRANICA_REGION=<YOUR_GRANICA_CLUSTER_REGION>
# Optional if not running on an ec2 instance to force read from a read-replica in this az
export GRANICA_AZ_ID=<AZ_ID>
```

For backwards compatibility, the following environment variables are supported

```
BOLT_CUSTOM_DOMAIN --> GRANICA_CUSTOM_DOMAIN
BOLT_REGION --> GRANICA_REGION
BOLT_AZ_ID --> GRANICA_AZ_ID
```

If the region and AZ environment variables aren't specified when running on an EC2 instance, the SDK will use the ec2 metadata api to fetch the instance's region and availability zone id.

## Example S3 GetObject

```python
import granica as boto3

s3_client = boto3.client('s3')
s3_client.put_object(Body="data", Bucket="BUCKET_NAME", Key="key")
response = s3_client.get_object(Bucket="BUCKET_NAME", Key="key")
obj = response['Body'].read()
```

## Debugging

Import the default logger and set its level to DEBUG

`logging.getLogger().setLevel(logging.DEBUG)`

## Tests

Basic integration tests are provided for the modified Session/Client interfaces. They must be run in an environment with a properly configured Granica deployment accessible.

```bash
python3 tests/tests.py
```
