

![1](https://github.com/soumilshah1995/run-aws-glue-locally-docker/assets/39345855/b8c77bad-1bcf-4216-b272-ac07db70dfae)

# Video Guide 
* https://www.youtube.com/watch?v=sk48a8B8Grs


# Steps 

### Step 1: Install AWS CLI 
Link : https://aws.amazon.com/cli/

### Step 2: Create a profile dev
```
aws configure --profile dev
```
![image](https://github.com/soumilshah1995/run-aws-glue-locally-docker/assets/39345855/748abc5e-1a71-4218-a578-9cff1b8312ef)

### Step 3: Pull Docker Image 
```
docker pull amazon/aws-glue-libs:glue_libs_4.0.0_image_01
```
### Step 4: Pull Docker Image 
```

docker run -it `
    -v C:\Users\XXXXXXX\.aws:/home/glue_user/.aws  `
    -v ./jupyter_workspace/:/home/glue_user/workspace/jupyter_workspace/  `
    -e AWS_PROFILE=dev  `
    -e DISABLE_SSL=true  `
    --rm -p 4040:4040 `
    -p 18080:18080  `
    -p 8998:8998 `
    -p 8888:8888 `
    --name glue_jupyter_lab amazon/aws-glue-libs:glue_libs_4.0.0_image_01 /home/glue_user/jupyter/jupyter_start.sh

```
###  Step 4: Head to http://localhost:8888/

###  Step 5: Execute your Glue Code in Jupyter Notebook
```

try:
    import os, uuid, sys, boto3, time, sys
    from pyspark.sql.functions import lit, udf
    import sys
    from pyspark.context import SparkContext
    from pyspark.sql.session import SparkSession
    from awsglue.transforms import *
    from awsglue.utils import getResolvedOptions
    from pyspark.context import SparkContext
    from awsglue.context import GlueContext
    from awsglue.job import Job
except Exception as e:
    print("Modules are missing : {} ".format(e))

# Create a Spark context and Glue context
sc = spark.sparkContext
glueContext = GlueContext(sc)
job = Job(glueContext)
logger = glueContext.get_logger()

file_name = "s3://athena-examples-us-east-1/notebooks/yellow_tripdata_2016-01.parquet"

glue_df = glueContext.create_dynamic_frame.from_options(
            format_options={},
            connection_type="s3",
            format="parquet",
            connection_options={
                "paths": [file_name],
                "recurse": True,
            },
        )
glue_df.toDF().show(3)

```

#### If you Enjoyed tutoirals please make sure to give a Star  on Github 

References 
* https://docs.aws.amazon.com/glue/latest/dg/aws-glue-programming-etl-libraries.html


