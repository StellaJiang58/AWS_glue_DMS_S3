# AWS_Project

## Challenge 1: INVALID_ARGUMENT_ERROR

**Error Category:** INVALID_ARGUMENT_ERROR  
**Failed Line Number:** 10  
**Error Message:** GlueArgumentError: the following arguments are required: `--s3_fileName`, `--s3_bucket`, `--s3_dst_folder`

### Solutions
1. **Manually Insert Parameters:**
   Add the required parameters in the Job Parameters console.
   <img width="598" alt="Screenshot 2025-04-03 at 3 52 37 PM" src="https://github.com/user-attachments/assets/de5a7dd3-394b-4fae-876c-6040efb8d842" />

   
3. **Change Arguments in Code:**
   Change the arguments dictionary from:
   ```python
   Arguments = {
       '--JOB_NAME': job_name,
       '--s3_fileName': file_name,
       '--s3_bucket': bucket_name,
       '--s3_dst_folder': destination_folder
   }
   to
   Arguments = {
       'JOB_NAME': job_name,
       's3_fileName': file_name,
       's3_bucket': bucket_name,
       's3_dst_folder': destination_folder
   }

## Challenge 2: NO LOGs FOUND in CLOUNDWATCH
The glue job run successfully. But I didn't see the expected fild in S3 destination folder(dst_folder). And also didn't see logs in glue_logs folder.

### Solutions

