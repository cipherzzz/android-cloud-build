# Android CI Build - Google Cloud 

## Thanks
A ton of the code in this repo was lifted from [this article](https://medium.com/dailymotion/run-your-android-ci-in-google-cloud-build-2487c8b70ccf) - thanks to Martin Bonnin

### The Build Container
This project is used to setup a new Google Cloud project to be able to run an Android CI build. Essentially, we are doing the following:
* Creating a docker image that has all of our dependencies in it(Android SDK, etc)
* Pushing that container to the Google project so that we can reuse it to build our Android apps
* Creating a `cache` storage bucket for gradle so that we can reuse already downloaded dependencies

The above steps only need to be done once per project. 

#### The Deets
* Install the Google Cloud SDK
* Create a Project
* Enable Cloud Build API from your project page (https://console.cloud.google.com/cloud-build/builds?project=[PROJECT_ID])

```  
# Login and set project
gcloud auth login
gcloud config set project <project name>

# Create gradle cache dir
gsutil mb gs://gradle_cache
_[PROJECT_ID]

# Create apk artifacts directory
gsutil mb gs://apk_[PROJECT_ID]

# Clone, build, and deploy the android builder docker image
git clone https://github.com/cipherzzz/android-cloud-build 
cd android-cloud-build-sample/cloud-builder
gcloud builds submit --config cloudbuild.yaml
```

You should now have a docker image installed in your project that you can use to build your client android app. See the next section for how to do this. 
***

### The Android App

The steps below should be done for every android app you want to build in your Google Cloud project

* Copy the `cloudbuild_app.yaml` file from the root of this project to the root directory of your android app
* Commit and push to your github repo
* Create a build trigger using your android repo on the branch you committed your `cloudbuild_app.yaml` file to. You really just need to specify the branch and your cloudbuild file as being `cloudbuild_app.yaml`.
* Manually trigger the build and check the `gs://apk_[PROJECT_ID]` bucket for the builds

This should be all you need to get your Android CI builds going in the Google Cloud! 