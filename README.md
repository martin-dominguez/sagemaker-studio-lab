# Develop your ML project with Amazon SageMaker
## Lab 3
**If you didn't complete LAB 2:**
 1. In your Dataflow go to the last transformation and click on `Add a destination`
 2. Complete parameters:
    * Dataset name: `bank-additional-transformed`
    * File type: CSV
    * Delimiter: Comma
    * Compression: None
    * Amazon S3 Location: Browse your `sagemaker-us-east-1-xxxxxxx/sagemaker/DEMO-xgboost-dm` bucket and folder
 3. Create Job. Keep the default setting and wait for it to finish. It'll take around 7 minutes tansform the whole dataset.
 4. In `feature_store_xgboost_direct_marketing_sagemaker.ipynb` notebook, go to `cell 05` and add a new cell.
 5. Copy the following code:
    ```
    import dask.dataframe as dd
    dataset_url = 's3://'+bucket+'/'+prefix+'/bank-additional-transformed.csv'
    model_data_dask = dd.read_csv(dataset_url)
    model_data = model_data_dask.compute()
    ```
 6. Ignore `cell 06` and continue executing the rest of the Notebook's cells
## Lab 4
**instance_type is a pipeline variable**
Although the value is not valid, SageMaker will get the good value from the `default_value` attribute. Anyway, if you want to avoid this message change `processing_instance_type` to `processing_instance_type.default_value`

### Define a Condition Step to Check Accuracy and Conditionally Register a Model in the Model Registry 

**JsonGet has been deprecated:**
```
from sagemaker.workflow.conditions import ConditionGreaterThanOrEqualTo
from sagemaker.workflow.condition_step import (ConditionStep)
from sagemaker.workflow.functions import JsonGet

cond_lte = ConditionGreaterThanOrEqualTo(  # You can change the condition here
        left=JsonGet(
            step_name=step_eval.name,
            property_file=evaluation_report,
            json_path="binary_classification_metrics.accuracy.value",  # This should follow the structure of your report_dict defined in the evaluate.py file.
        ),
        right=0.8,  # You can change the threshold here
)

step_cond = ConditionStep(
    name="MarketingAccuracyCond",
    conditions=[cond_lte],
    if_steps=[step_register],
    else_steps=[]
)
```
