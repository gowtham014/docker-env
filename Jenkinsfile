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
      - '/busybox/cat'
      tty: true
  volumeMounts:
    - name: kaniko-secret
      mountPath: '/kaniko/.docker'
volumes:
- name: kaniko-secret
  projected:
    sources:
    - secret:
        name: docker-cred
        items:
          - key: dockerconfigjson
            path: config.json
"""
        }
    }
    stages  {
        stage('build with kaniko'){
          environment{
          //  DOCKER_USERNAME = credentials('docker-credentials').username
          //  DOCKER_PASSWORD = credentials('docker-credentials').password
            KANIKO_DOCKER_CREDS=credentials('docker-credentials')
            DOCKER_CONFIG = '/kaniko/.docker'
          }
            steps {
              git url:"https://github.com/gowtham014/docker-env.git",
              branch: "main"
                container('kaniko'){
                  //  sh "echo '{\"auths\":{\"docker.io\":{\"username\":\"$DOCKER_USERNAME\",\"password\":\"$DOCKER_PASSWORD\",\"email\":\"\"}}}' > /kaniko/.docker/config.json"
                  // sh "mkdir -p $DOCKER_CONFIG && touch $DOCKER_CONFIG/config.json"
                  // echo " $KANIKO_DOCKER_CREDS> $DOCKER_CONFIG/config.json "
                  sh "/kaniko/executor -f `pwd`/Dockerfile -c `pwd` --destination=gowtham014/docker-env:1.0"
                    }
                }
        }
    }
}

