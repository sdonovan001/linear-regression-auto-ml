# Model Serving (Locally)
While leveraging AutoML enables you to train a model without writing code, it seems wrong that the model I paid Google to produce for me is now subject to vendor lock in when it comes time to serve my model.  This should be my intellectual property that I can do with as I please.  Well... it is... and you can!


<kbd>
<img src="/images/auto-ml-ip.png" alt="On Nooo!" width="800" height="550">
</kbd>

<p> </p>

Serving a model locally when you trained it on Vertex AI is pretty simple.  The most complicated aspect is ensuring the container you are using for serving locally has all the same versions of the components present in the container you used for training.  This is critical because the TF SavedModel format changes slightly over time.  You must ensure the format of the model you saved is compatible with the format the serving container is expecting.  While that could be a huge sink hole of time... I'll show you how to easily side step that problem.  The steps for serving a Vertex AI trained model locally are:

<p> </p>

* Export the trained model to a GCS storage bucket.
* Download the model artifacts locally.
* Pull the appropriate Docker container for model serving.
* Run the container... and serve predictions.

#### Exporting the Model
[![something is broken](/images/video2.png)](https://www.youtube.com/embed/OggyXfDCzWY "Model Export")

#### Download... Pull... Run... Serve
```
# Perform some cleanup on the path of the downloaded model artifacts to make bind mounting
# your model into a docker container easier...
mkdir tf-saved-model
cp -r ./model*/tf*/20*/* ./tf-saved-model

# Find exactly what container was used to train your model in GCP AutoML and assign it to a variable...
container=$(cat ./tf-saved-model/env* | jq .| grep container | awk '{ print $2 }' | sed 's/,//' | sed 's/"//g')

# Pull the container locally (this will take a while so be patient)...
docker pull ${container}

# Run prebuilt GCP container (the container used to train the model) to serve the model...
docker run --platform linux/amd64 -v `pwd`/tf-saved-model:/models/default -p 8080:8080 -it us-docker.pkg.dev/vertex-ai/automl-tabular/prediction-server:prod

# Test the container in another window....
curl -X POST -d @test-prediction.json http://localhost:8080/predict | jq .
...
{
  "predictions": [
    {
      "value": 61.39912796020508,
      "lower_bound": 52.01882553100586,
      "upper_bound": 75.47786712646484
    },
    {
      "value": 15.74298095703125,
      "lower_bound": 11.935120582580566,
      "upper_bound": 22.818044662475586
    },
    {
      "value": 23.56092071533203,
      "lower_bound": 18.869943618774414,
      "upper_bound": 39.19050598144531
    }
  ]
}

```
