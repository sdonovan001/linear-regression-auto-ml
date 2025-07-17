# Linear Regression With AutoML (No Coding Necessary!)
AutoML, or Automated Machine Learning, automates the process of building and deploying machine learning models, making it more accessible and efficient, even for those without extensive data science experience. It handles tasks like data preparation, feature selection, model selection, and hyperparameter tuning automatically. In this repo I'll show you how you can train and serve a predictive linear regression model, similar to what we did in this [repo](https://github.com/sdonovan001/ml-linear-regression/blob/main/README.md), but without writing a single line of code!

[![something is broken](/images/video1.png)](https://www.youtube.com/embed/7P9HQUvk2DA "Overview")

### AutoML Pipeline
A standard machine learning (ML) pipeline is a systematic workflow that streamlines the entire process of building, training, deploying, and managing ML models. The AutoML Pipeline is very similar to a standard ML Pipeline.  The big difference is with a standard ML Pipeline you have to write model training code and manually iterate on training / validating / tuning to arrive at a good predictive model, with AutoML you need only answer a few prompts on the GCP console... and wait for the system to do all of the leg work for you.

<img src="/images/auto-ml-pipeline.png" alt="On Nooo!" witdh="400" height="300">

### Data Preparation
Data preparation in machine learning is the process of cleaning, transforming, and organizing raw data into a format suitable for training machine learning models. This crucial step ensures that the data is accurate, consistent, and relevant, leading to more reliable and effective models. In this example, most of that work has already been done for us.  To use AutoML to train our model, we need only upload our taxi fare data to a GCP storage bucket and create a Vertex AI dataset.  The video below walks you through doing that.

[![something is broken](/images/video2.png)](https://www.youtube.com/embed/vCp4Ih029ds "Data Prep")

### Model Training
Model training is the process of teaching a machine learning (ML) model to make accurate predictions or decisions by exposing it to data. This involves feeding the model data, evaluating its performance, and adjusting its internal parameters (like weights and biases) to minimize errors and improve its ability to generalize to new, unseen data.  Now that we have created a VertexAI dataset to train our model with, we need to provide AutoML with some configuration information so it can train our model.  The video below walks you through those steps.

[![something is broken](/images/video2.png)](https://www.youtube.com/embed/6U-NFiU6f4A "Model Training")

### Model Serving
Model serving is the process of making a trained machine learning model accessible for making predictions in a production environment. It involves deploying the model, typically behind an API, so that applications or users can interact with it and receive real-time or batch predictions. Essentially, it's the bridge between a trained model and its practical application. 

Since we have trained our model in the cloud, the easiest way to expose it as a service is to stand up an AutoML Cloud Endpoint to serve predictions.  While it may be easy leveraging cloud computing resources cost money... so I will also show you how to serve you model locally on compute resources that you already own.

* [Model Serving with Cloud Endpoints](/cloud-serving/README.md)
* [Model Serving Locally with Docker](/local-serving/README.md)
