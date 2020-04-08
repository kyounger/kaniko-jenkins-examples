pipeline {
    agent {
        kubernetes {
            defaultContainer 'kaniko'
            yaml """
kind: Pod
metadata:
  name: kaniko
spec:
  containers:
  - name: kaniko
    image: gcr.io/kaniko-project/executor:debug-v0.19.0
    imagePullPolicy: Always
    command:
    - /busybox/cat
    tty: true
"""
        }
    }
    environment {
        IMAGE_PUSH_DESTINATION="kyounger/kaniko-jenkins:jenkins-secret-declarative"
    }
    stages {
        stage('Build with Kaniko') {
            steps {
                checkout scm
                container(name: 'kaniko', shell: '/busybox/sh') {
                    withCredentials([file(credentialsId: 'docker-credentials', variable: 'DOCKER_CONFIG_JSON')]) {
                        withEnv(['PATH+EXTRA=/busybox']) {
                            sh '''#!/busybox/sh
                                cp $DOCKER_CONFIG_JSON /kaniko/.docker/config.json
                                /kaniko/executor --context `pwd` --destination $IMAGE_PUSH_DESTINATION
                            '''
                        }
                    }
                }
            }
        }
    }
}
