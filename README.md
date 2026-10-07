<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# Encrypt Data with AWS KMS

**Project Link:** [View Project](https://nextwork.ai/projects/716e7f81-84d1-51d6-8945-d4860caebf77)

**Author:** Komeezzy  
**Email:** komonservices@gmail.com

---

![Image](https://nextwork.ai/content_turquoise_clever_cumin/uploads/716e7f81-84d1-51d6-8945-d4860caebf77_w0x1y2z3)

## Introducing Today's Project!

In this project, I will demonstrate data encryption with AWS KMS. The goal is to protect sensitive information, customer data, or even national secrets.

### Tools and concepts

Services I used include AWS KMS, Amazon DynamoDB and AWS IAM. Key concepts I learnt include encryption, customer managed keys (CMKs), symmetric vs asymmetric keys, key policies, key administrators vs key users, transparent data encryption, and encryption at rest.

### Project reflection

This project took me approximately 1 hour 30 minutes. The most challenging part was understanding the difference between DynamoDB permissions and KMS key permissions. It was most rewarding when I finally understood how the AWS services worked together, especially since I was nervous to try this project at first.

I chose to do this project today because it will help me with my Cloud Security path. Something that would make learning with NextWork even better is letting us submit a version of our own and discuss it with other students.

## Encryption and KMS

Encryption is the process of transforming plain text into a jumbled up format called ciphertext, which only authorized users with the right key can turn back into readable data. Companies and developers do this so that even if an outsider gets through, they can't read the data. Encryption keys are the codes the encryption algorithm uses to lock and unlock data, and they're used to protect data at rest and in transit across cloud applications.

AWS KMS is a secure vault for my encryption keys. Key management systems are important because they let us create, manage, and use the encryption keys that protect the data in our AWS resources.

Encryption keys are broadly categorized as symmetric (one key both encrypts and decrypts) and asymmetric (a public key encrypts and a private key decrypts). I set up a symmetric key because it's faster and more efficient for encrypting large amounts of data, which suits a DynamoDB table where only AWS services, not outside parties, need to use the key.

![Image](https://nextwork.ai/content_turquoise_clever_cumin/uploads/716e7f81-84d1-51d6-8945-d4860caebf77_a2b3c4d5)

## Encrypting Data

My encryption key will safeguard data in DynamoDB, which is one of AWS's database services. It's a fast and flexible way to store data.

The different encryption options in DynamoDB include an AWS owned key, an AWS managed key and a customer managed key. Their differences come down to control: DynamoDB fully manages the key, AWS manages it while you can view its usage, or you create and manage it yourself. I selected a customer managed key because I want full control over the key's policy and who can use it.

![Image](https://nextwork.ai/content_turquoise_clever_cumin/uploads/716e7f81-84d1-51d6-8945-d4860caebf77_q8r9s0t1)

## Data Visibility

Rather than controlling who has access to the key, KMS manages user permissions by using the key policy to define which specific actions (like kms:Encrypt or kms:Decrypt) each user or role can perform with it. So my test user could reach the table, but without kms:Decrypt the data stayed locked.

Despite encrypting my DynamoDB table, I could still see the table's items because my IAM Admin user has permission to use the KMS key. DynamoDB uses transparent data encryption, which means it automatically decrypts the data for authorized users when they request it, so the data stays encrypted at rest but is still readable for anyone with the right permissions.

![Image](https://nextwork.ai/content_turquoise_clever_cumin/uploads/716e7f81-84d1-51d6-8945-d4860caebf77_c0d1e2f3)

## Denying Access

I configured a new IAM user to test whether someone without KMS key permissions can read the encrypted data. The permission policies I granted this user are AmazonDynamoDBFullAccess, but not any permissions to use my KMS key (such as kms:Decrypt).

When I checked my DynamoDB table as the test user, I got an access denied error saying I didn't have permission to perform kms:Decrypt. The user could reach the table through DynamoDB permissions, but couldn't read the items because they had no permission to use the KMS key.

![Image](https://nextwork.ai/content_turquoise_clever_cumin/uploads/716e7f81-84d1-51d6-8945-d4860caebf77_w0x1y2z3)

## EXTRA: Granting Access

To let my test user use the encryption key, I added nextwork-kms-user as a key user in the KMS console. My key's policy was updated to include the user's ARN in the "Allow use of the key" statement, which grants kms:Encrypt, kms:Decrypt, kms:ReEncrypt*, kms:GenerateDataKey* and kms:DescribeKey.

To verify my test user's access, I went back to the incognito window where I was logged in as nextwork-kms-user and refreshed the Explore items page in DynamoDB. This time, the item with id 1 loaded with no access denied error, which confirmed that the updated key policy gave the test user permission to decrypt the data.

Encryption secures data instead of the resource or service it's stored in. I could combine encryption with IAM policies and security groups to build defense in depth, so even if someone gets past the access controls, they still can't read the data without the decryption key.

![Image](https://nextwork.ai/content_turquoise_clever_cumin/uploads/716e7f81-84d1-51d6-8945-d4860caebf77_feffb2fb8)

---

*Built with [NextWork](https://nextwork.ai) - [View this project](https://nextwork.ai/projects/716e7f81-84d1-51d6-8945-d4860caebf77)*
