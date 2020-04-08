## How to store crendentials for kaniko to use to push image

### How to create a kubernetes secret with docker credentials
Replace the empty string here with your user/pass/email
```
kubectl create secret docker-registry docker-credentials \
    --docker-username=""  \
    --docker-password="" \
    --docker-email=""
```

### How to create a Jenkins secret file that contains config.json
1. Navigate to credentials
2. Navigate to your domain
3. Add Credentials
    * Kind: Secret File
    * Scope: select proper scope
    * File: select the file on your local filesystem
    * ID: for the example Jenkinsfile in this repo, use "docker-credentials"
    * Description: enter something meaningful
