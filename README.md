# Linear Regression With AutoML (No Coding Necessary!)
AutoML, or Automated Machine Learning, automates the process of building and deploying machine learning models, making it more accessible and efficient, even for those without extensive data science experience. It handles tasks like data preparation, feature selection, model selection, and hyperparameter tuning automatically. In this repo I'll show you how you can train and serve a predictive linear regression model, similar to what we did in this [repo](https://github.com/sdonovan001/ml-linear-regression/blob/main/README.md), but without writing a single line of code!

[![something is broken](/images/video1.png)](https://www.youtube.com/embed/7P9HQUvk2DA "Overview")

### AutoML Pipeline
The AutoML Pipeline is very similar to a standard ML Pipeline.  The big difference is with a standard ML Pipeline you have to write model training code and manually iterate on training / validating / tuning to arrive at a good predictive model, with AutoML you need only answer a few prompts on the GCP console... and wait for the system to do all of the leg work for you.

<img src="/images/auto-ml-pipeline.png" alt="On Nooo!" witdh="400" height="300">

### Data Preparation
To use AutoML to train our model, we must first upload our data to a GCP storage bucket and create a Vertex AI dataset.  The video below walks you through doing that.

[![something is broken](/images/video2.png)](https://www.youtube.com/embed/vCp4Ih029ds "Data Prep")

### Model Training
Now that we have a dataset to use to train our model, we need to provide AutoML with some configuration information so it can train our model.  The video below walks you through those steps.

[![something is broken](/images/video2.png)](https://www.youtube.com/embed/6U-NFiU6f4A "Model Training")

---
### Model Serving (Cloud)
Now that we have trained our model, we can easily stand up an AutoML Endpoint to serve our model predictions.  The video below walks you through those steps.

[![something is broken](/images/video2.png)](https://www.youtube.com/embed/PsSKW01U2Dk "Model Serving")

```
# To hit the model we just deployed to an endpoint from the command line... follow the steps below.

# Authenticate with GCP...
gcloud auth application-default login

# Set a default region for Vertex AI...
gcloud config set ai/region us-east1

# Save a few values for later use...
PROJECT_ID=$(gcloud config get-value project)
REGION=$(gcloud config get-value ai/region)
ENDPOINT_ID=$(gcloud ai endpoints list | grep -v ENDPOINT | grep cab-fare | awk '{print $1}')

# Look at request payload...
cat test-prediction.json

{
  "instances": [
    { "TRIP_MILES": "24.7", "TRIP_MINUTES": "40.66" },
    { "TRIP_MILES": "4.66", "TRIP_MINUTES": "16.95" },
    { "TRIP_MILES": "8.23", "TRIP_MINUTES": "17.41" }
  ]
}

# Send test-prediction.json to our Vertex AI endpoint...
curl -X POST \
     -H "Authorization: Bearer $(gcloud auth print-access-token)" \
     -H "Content-Type: application/json; charset=utf-8" \
     -d @test-prediction.json \
     "https://${REGION}-aiplatform.googleapis.com/v1/projects/${PROJECT_ID}/locations/${REGION}/endpoints/${ENDPOINT_ID}:predict"

# This should return something similar to...
{
  "predictions": [
    {
      "lower_bound": 52.018825531005859,
      "upper_bound": 75.477867126464844,
      "value": 61.399127960205078
    },
    {
      "value": 15.74298095703125,
      "lower_bound": 11.93512058258057,
      "upper_bound": 22.818044662475589
    },
    {
      "lower_bound": 18.86994743347168,
      "value": 23.560920715332031,
      "upper_bound": 39.190505981445312
    }
  ],
  "deployedModelId": "5247329083607482368",
  "modelDisplayName": "cab-fare",
  "modelVersionId": "1"
}

```
---
### Model Serving (Local)
While leveraging AutoML enables you to train a model without writing code, it seems wrong that the model I paied Google to produce for me is now subject to vendor lock in when it comes time to serve my model.  This should be my intellectual property that I can do with as I please.  Well... it is... and you can!


<kbd>
<img src="/images/auto-ml-ip.png" alt="On Nooo!" width="800" height="550">
</kbd>

<p> </p>

Serving a model locally when you trained it on Vertex AI is pretty simple.  The most complicated aspect is ensuring the container you are using for serving locally has all the same versions of the components present in the container you used for training.  This is critical because the TF SavedModel format changes slightly over time.  You must ensure the format of the model you saved is compatible with the format the serving container is expecting.  While that could be a huge sink hole of time... I'll show you how to easily side step that problem.  The steps for serving a Vertex AI trained model locally are:

<p> </p>

* Export the trained model to a GCS storage bucket.
* Download the model artifacts locally.
* Pull the appropriate Docker container for model serving.
* Run the container... serve predictions.

#### Exporting the Model
[![something is broken](/images/video2.png)](https://www.youtube.com/embed/PsSKW01U2Dk "Model Serving")

#### Download... Pull... Run... Serve
```
bla...bla...bla...


```
