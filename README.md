# Linear Regression With AutoML
AutoML, or Automated Machine Learning, automates the process of building and deploying machine learning models, making it more accessible and efficient, even for those without extensive data science experience. It handles tasks like data preparation, feature selection, model selection, and hyperparameter tuning automatically. In the videos contained in this repo, I'll show you how you can train and serve a predictive linear regression model, similar to what we did in this [repo](https://github.com/sdonovan001/ml-linear-regression/blob/main/README.md), but without writing a single line of code!

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

### Model Serving (Cloud)
Add video for model deployment
```
# Authenticate with GCP...
gcloud auth application-default login

# Set a default region for Vertex AI...
gcloud config set ai/region us-east1

# Save a few values for later use... 
REGION=$(gcloud config get-value ai/region)
ENDPOINT_ID=$(gcloud ai endpoints list | grep -v ENDPOINT | grep cab-fare | awk '{print $1}')

# Look at request payload...
cat test-prediction.json

{
  "instances": [
    { "TRIP_MILES": "24.7", "TRIP_MINUTES": "40.66" },
    { "TRIP_MILES": "4.66", "TRIP_MINUTES": "16.95" },
    { "TRIP_MILES": "8.23", "TRIP_MINUTES": "17.41" },
  ]
}

# Send test-prediction.json to our Vertex AI endpoint...
curl -X POST \
     -H "Authorization: Bearer $(gcloud auth print-access-token)" \
     -H "Content-Type: application/json; charset=utf-8" \
     -d @test-prediction.json \
     "https://${REGION}-aiplatform.googleapis.com/v1/projects/${PROJECT_ID}/locations/${REGION}/endpoints/${ENDPOINT_ID}:predict"

```

### Model Serving (Local)
