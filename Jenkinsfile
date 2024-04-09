pipeline{
    agent{
        kubernetes{
            yaml """
kind: Pod
spec:
containers:
- name: kaniko
  image: gcr.io/kaniko-project/executor:debug
  imagePullPolicy: Always
  command:
    - sleep
  args:
    - 9999999
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
            steps {
                container('kaniko'){
                    sh ' /kaniko/executor --context `pwd` --destination gowtham014/docker-env:1.0'
                }
            }
        }
    }
}