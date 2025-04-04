# AWS_Project

## DMS Setup(get the original csv.file)  
1. **Create DMS target endpoint**  
   Create DMS help us to transfer data to Amazon S3
   https://us-east-1.console.aws.amazon.com/dms/v2/home?region=us-east-1#endpointDetails/feb-target-endpoint-sijun

3. **Create DMS task**    
   source is feb-source-db target is feb-target-endpoint-sijun I create at last step.
   When I run the task, the data will be transfromed from Mysql to S3 bucket.
   Once I did insert or delect, the table statistic will catch these changes.
   https://us-east-1.console.aws.amazon.com/dms/v2/home?region=us-east-1#taskDetails/feb-dms-task-sijun

## Lambda Trigger setting  
1. **Create lambda function**  
   create a lambda function called feb-s3-trigger-glue-sijun and then create a tigger with S3 and set the prefix is "staging_folder/world/Persons_sijun/"
   https://us-east-1.console.aws.amazon.com/lambda/home?region=us-east-1#/functions/feb-s3-trigger-glue-sijun? subtab=triggers&tab=code

3. **Create Glue Job**  
   Create code with a script editor and spark engine. And set there S3 file path for glue_script, glue_temp, and
   glue_logs in advanced properties options.
   Writing code under "script" option and run it to see details under "runs" option.
   https://us-east-1.console.aws.amazon.com/gluestudio/home?region=us-east-1#/editor/job/feb-glue-sijun/script

### Challenge 1: INVALID_ARGUMENT_ERROR
**Error Category:** INVALID_ARGUMENT_ERROR  
**Failed Line Number:** 10  
**Error Message:** GlueArgumentError: the following arguments are required: `--s3_fileName`, `--s3_bucket`, `--s3_dst_folder`

### Solutions
1. **Manually Insert Parameters:**
   Add the required parameters in the Job Parameters console.
   <img width="598" alt="Screenshot 2025-04-03 at 3 52 37 PM" src="https://github.com/user-attachments/assets/de5a7dd3-394b-4fae-876c-6040efb8d842" />

   Didn't use it the name for file_name could be determined. In addtion, even thought the job shows succeeded, but I
   didn't see any logs and tables in dest_folder 
   
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

before it shows success but after I rerun it, it show failed. So I didn't use this method

Final solution, just directly assign the value for these variables.

### Challenge 2: Unexpected removing rows
   The first three rows were removed for file startwith "LOAD"; and 1 row is remove from file not startwith "LOAD";
   I didn't figure out this issue yet.
