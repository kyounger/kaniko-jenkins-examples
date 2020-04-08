def label = "kaniko-${UUID.randomUUID().toString()}"

podTemplate(name: 'kaniko', label: label, yaml: """
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
""") {
  node(label) {
    def IMAGE_PUSH_DESTINATION="kyounger/kaniko-jenkins:jenkins-secret"
    stage('Build with Kaniko') {
        checkout scm
        container(name: 'kaniko', shell: '/busybox/sh') {
            withCredentials([file(credentialsId: 'docker-credentials', variable: 'DOCKER_CONFIG_JSON')]) {
                withEnv(['PATH+EXTRA=/busybox',"IMAGE_PUSH_DESTINATION=${IMAGE_PUSH_DESTINATION}"]) {
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
