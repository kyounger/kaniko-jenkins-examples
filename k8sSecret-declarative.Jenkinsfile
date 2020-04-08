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
    volumeMounts:
      - name: jenkins-docker-cfg
        mountPath: /kaniko/.docker
  volumes:
  - name: jenkins-docker-cfg
    projected:
      sources:
      - secret:
          name: docker-credentials
          items:
            - key: .dockerconfigjson
              path: config.json
"""
        }
    }
    environment {
        IMAGE_PUSH_DESTINATION="kyounger/kaniko-jenkins:k8s-secret-declarative"
    }
    stages {
        stage('Build with Kaniko') {
            steps {
                checkout scm
                container(name: 'kaniko', shell: '/busybox/sh') {
                    withEnv(['PATH+EXTRA=/busybox']) {
                        sh '''#!/busybox/sh
                            /kaniko/executor --context `pwd` --destination $IMAGE_PUSH_DESTINATION
                        '''
                    }
                }
            }
        }
    }
}
