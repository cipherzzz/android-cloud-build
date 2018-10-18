# Dockerfile.vmutils
The Dockerfile.vmutils is meant to be used as a general docker image to run commands such as `curl` in a Google cloudbuild. More can be added by updating the Dockerfile. 

Installing the docker image on google cloud

```
# Build the image
docker build -f Dockerfile.vmutils -t gcr.io/<project-id>/vmutils .

# Push to google cloud
gcloud docker -- push gcr.io/<project-id>/vmutils

# Usage in the cloudbuild.yaml example
- name: 'gcr.io/$PROJECT_ID/vmutils'
  args: ['bash', './curl_script.sh']
```