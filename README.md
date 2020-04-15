# Jenkins kaniko examples for building images in kubernetes

Available are both declarative and scripted options, and using k8s secrets or Jenkins Credentials for storing config.json docker credentials.

## How to store crendentials for kaniko to use to push image

### How to create a kubernetes secret with docker credentials
Replace the empty string here with your user/pass/email
```
kubectl create secret docker-registry docker-credentials \
    --docker-username=""  \
    --docker-password="" \
    --docker-email=""
```

### Create config.json for use elsewhere
You can use the same kubectl command and just use it locally to generate the file via --dry-run
```
kubectl create secret docker-registry docker-credentials \
    --docker-username=""  \
    --docker-password="" \
    --docker-email="" --dry-run=client -o jsonpath='{.data.\.dockerconfigjson}' | base64 -D > config.json
```

### How to add config.json as secret file credential
1. Navigate to credentials
2. Navigate to your domain
3. Add Credentials
    * Kind: Secret File
    * Scope: select proper scope
    * File: select the file on your local filesystem
    * ID: for the example Jenkinsfile in this repo, use "docker-credentials"
    * Description: enter something meaningful
