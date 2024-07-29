---
title: Security
icon: material/cloud-key-outline
---

# Security

Recce Cloud is designed to be secure by default. We take security seriously and consider the data privacy from the beginning of the design phase. In this document, we will describe how we handle your data on Recce Cloud.


## State File in Recce Cloud
When users execute `recce run` or `recce server` with option `--cloud`, Recce will upload the state file into the Recce Cloud. In the meanwhile, Recce will also request users provide the encryption password by option `--password` or `-p`. Recce will use the password to encrypt the state file when uploading and decrypt the state file when downloading. The password will not be stored in Recce Cloud, so users need to keep it safe.

### How Recce Encrypts the State File
Once users choose to upload the state file to Recce Cloud, Recce will store the state file in the cloud storage. Recce Cloud will use AWS S3 to store the state file and enable the [server-side encryption with customer-provided keys (SSE-C)](https://docs.aws.amazon.com/AmazonS3/latest/userguide/ServerSideEncryptionCustomerKeys.html). Recce Cloud will use the encryption password provided by users to encrypt the state file before uploading it to the S3 bucket. The encryption algorithm is AES-256. The state file will be decrypted with the same password when downloading it from the S3 bucket. 

![Flow of encrypt/decrypt state file](../../../assets/images/recce-cloud/sse-c.png)

Based on the above flow chart, the state file will be encrypted and decrypted on the AWS S3 side. Recce Cloud server will only in charge of generating the pre-signed URL for uploading and downloading the state file. Which means Recce Cloud server will not have access to the state file content and the password key. 
