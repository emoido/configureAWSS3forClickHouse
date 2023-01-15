
Configure AWS S3 Object Store For using  as ClickHouse disk

- Under AWS IAM, select _ADD Users_

- Enter username with credential type to Access key - Programmatic access

![add_user_01](https://user-images.githubusercontent.com/45426637/212554092-95191925-f6b3-4621-8f50-66863450a91f.png)

- Do not add user to any groups or do not add tags to user(acept default options until step 5)

- In step 5, note down the access key and secret access key for use in the ClickHouse storage configuration.

![add_user_05](https://user-images.githubusercontent.com/45426637/212554701-58f778b5-116f-4424-b793-290d24351ec4.png)

- Click created user from the _Users_ screen

![add_user_06](https://user-images.githubusercontent.com/45426637/212554935-dd8cc246-471d-4158-8a76-2a8a54b1e64f.png)

- Copy the ARN (Amazon Resource Name) and save it for use when configuring the access policy for the bucket.

![copy_user_arn](https://user-images.githubusercontent.com/45426637/212555055-5c075790-f162-42c4-af2d-ae5b1c6a85f3.png)

- Now it is time to create an S3 bucket; for that reason, select "Create Bucket" in S3.

![Screenshot 2023-01-15 at 20 22 04](https://user-images.githubusercontent.com/45426637/212556640-5bf2e3d1-e3b3-49b3-b27d-7ccb8d7923ca.png)


- At that point, enter a bucket name(all other options must be default) and enter "Create Bucket" at the bottom of the page

- After creating bucket, you should see your new bucket under "_Buckets_"

- Check your bucket, copy the ARN (it is used in creating storage policy at clickhouse server)

![copy_bucket_arn](https://user-images.githubusercontent.com/45426637/212556492-383d2cb5-7b11-4757-9834-7abddb6f5a6b.png)

- Select the bucket's link in the "Buckets" section and select "Create Folder" for creating a folder to use as ClickHouse storage.

![Screenshot 2023-01-15 at 20 26 25](https://user-images.githubusercontent.com/45426637/212556819-360f53b3-deed-422d-95d8-d13463f5a978.png)

- Enter a folder name and select Create folder.(other options must be default)

![create_folder](https://user-images.githubusercontent.com/45426637/212556979-fb0e295d-acfa-461d-bbbd-58aaf444b93e.png)

- Check the created folder and copy the URL for using in the ClickHouse storage configuration.

![Screenshot 2023-01-15 at 20 31 48](https://user-images.githubusercontent.com/45426637/212557028-ddd62c84-e446-427f-ba5f-90c75b61783c.png)

- Select created bucket and choose "Permissions" tab.

![Screenshot 2023-01-15 at 20 36 30](https://user-images.githubusercontent.com/45426637/212557292-19624f7e-3de7-4b22-a4ad-abadf9af7ff1.png)

- Click on the "Edit" button in the "Bucket Policy" section and add the policy above with your credentials and informations.

```sh
{
    "Version": "2012-10-17",
    "Id": "Policy1",
    "Statement": [
        {
            "Sid": "CHS3_1",
            "Effect": "Allow",
            "Principal": {
                "AWS": "arn:aws:iam::<YOUR_IAM_ID>:user/<YOUR_USER>"
            },
            "Action": "s3:*",
            "Resource": [
                "arn:aws:s3:::<YOUR_BUCKET>",
                "arn:aws:s3:::<YOUR_BUCKET>/*"
            ]
        }
    ]
}

```

