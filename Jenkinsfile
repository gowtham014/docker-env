pipeline{
    agent{
        kubernetes{
          defaultContainer 'jnlp'
            yaml """
apiVersion: v1
kind: Pod
metadata:
  labels:
    build-spec: kaniko
spec:
  containers:
    - name: kaniko
      image: gcr.io/kaniko-project/executor:debug
      imagePullPolicy: Always
      command:
      - /busybox/cat
      tty: true
  volumeMounts:
    - name: kaniko-secret
      mountPath: /kaniko/.docker
volumes:
- name: jenkins-docker-cfg
  projected:
    sources:
    - secret:
        name: docker-cred
        items:
          - key: .dockerconfigjson
            path: config.json
"""
        }
    }
    stages{
        stage('build with kaniko'){
          environment{
            KANIKO_DOCKER_CREDS=credentials('docker-credentials')
          }
            steps {
                container('kaniko'){
                  sh 'cp $KANIKO_DOCKER_CREDS /kaniko/.docker/config.json'
                  sh ' /kaniko/executor -f `pwd`/Dockerfile -c `pwd` --destination=gowtham014/docker-env:1.0'
                }
            }
        }
    }
}