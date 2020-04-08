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
""") {
  node(label) {
    def IMAGE_PUSH_DESTINATION="kyounger/kaniko-jenkins:k8s-secret"
    stage('Build with Kaniko') {
        checkout scm
        container(name: 'kaniko', shell: '/busybox/sh') {
            withEnv(['PATH+EXTRA=/busybox',"IMAGE_PUSH_DESTINATION=${IMAGE_PUSH_DESTINATION}"]) {
                sh '''#!/busybox/sh
                    /kaniko/executor --context `pwd` --destination $IMAGE_PUSH_DESTINATION
                '''
            }
        }
      }
    }
  }
