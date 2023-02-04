# Develop your ML project with Amazon SageMaker
## Lab 3 Files
Use this files if you didn't complete the Lab 2
1. Open a terminal in your SageMaker Studio
2. Copy the zip file locally in your Studio

    `curl -LJO https://github.com/martin-dominguez/sagemaker-studio-lab/raw/master/DEMO-xgboost-dm.tgz`

3. Uncompress it

    `tar xvfz DEMO-xgboost-dm.tgz`

4. Get the SageMaker S3 Bucket name (sagemaker-us-east-1-xxx)
5. Upload to the sagemaker bucket

    `aws s3 sync DEMO-xgboost-dm s3://sagemaker-us-east-1-497176641681/sagemaker/DEMO-xgboost-dm`

6. Execute `cell 01` and `cell 02` and then go to `cell 11`
