# AWS S3 and Terraform Exercise

This exercise is designed to give you hands-on experience with managing AWS S3 buckets 
through both the AWS Management Console and Terraform. It will cover creating and deleting 
buckets manually, uploading files, and then automating these processes with Terraform, including 
enabling encryption.

## Part 1: Using the AWS Management Console

### Step 1: Creating an S3 Bucket

1. Log in to the AWS Management Console with your credentials.
2. Navigate to the S3 service page.
3. Click on **Create bucket**.
4. For the bucket name, use a unique name that can be easily identified as yours (e.g., your AWS credentials or a variation of your name).
5. Select the ```eu-west-1``` region to you for the bucket.
6. Leave the default settings, and click **Create** at the bottom of the page.

### Step 2: Uploading a File to the Bucket

1. After creating your bucket, click on its name to open it.
2. Click on **Upload**.
3. Click on **Add files**, and select a file from your computer to upload.
4. After adding the file, click on **Upload** at the bottom of the page.

### Step 3: Deleting the Bucket

1. Navigate back to the main S3 page where all buckets are listed.
2. Select the bucket you created by clicking the checkbox next to its name.
3. Click on **Delete**.
4. Take not of the warning "This bucket is not empty", click the "Empty bucket" 
5. You will be asked to confirm the bucket deletion by entering its name. Do so, then click **Confirm**.

## Part 2: Automating with Terraform

You will do this part in your Cloud9 environment.

### Step 1: Writing Terraform Code for the S3 Bucket

Create a new Terraform configuration file (`s3_bucket.tf`) with the following content:

```hcl
provider "aws" {
  region = "your-region" # Example: us-east-1
}

resource "aws_s3_bucket" "my_bucket" {
  bucket = "your-unique-bucket-name"
  acl    = "private"
}
```

### Step 2: Initializing and Planning with Terraform

1. Open a terminal and navigate to the directory containing your Terraform configuration file.
2. Run `terraform init` to initialize the Terraform workspace.
3. Run `terraform plan` to see the actions Terraform will take based on your configuration.

### Step 3: Applying the Terraform Configuration

1. Run `terraform apply` and type `yes` when prompted to create the bucket.
2. Run `terraform plan` again, and see that there is no change 

### Step 4: Testing the Bucket

1. Manually upload a file to the bucket through the AWS Management Console, as described in Part 1, Step 2.

### Step 5: Enabling Encryption with Terraform

1. Modify the `s3_bucket.tf` file to enable server-side encryption by adding the following block inside the `aws_s3_bucket` resource:

```hcl
  server_side_encryption_configuration {
    rule {
      apply_server_side_encryption_by_default {
        sse_algorithm = "AES256"
      }
    }
  }
```

2. Run `terraform plan` and `terraform apply` to apply the encryption settings.

### Step 6: Emptying the Bucket Using the CLI

1. Use the AWS CLI to empty the bucket:

```sh
aws s3 rm s3://your-unique-bucket-name --recursive
```

### Step 7: Destroying the Bucket with Terraform

1. Run `terraform destroy` to remove the bucket and all its configurations.
