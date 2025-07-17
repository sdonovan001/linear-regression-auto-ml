# Model Serving (Cloud)
Now that we have trained our model, we can easily stand up an AutoML Endpoint to serve our model predictions.  Keep in mind that using cloud computing resources to serve your model costs money.  You should always perform a cost projection before deploying cloud resources.  The video below walks you through those steps of deploying a model to a Cloud Endpoint.

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
