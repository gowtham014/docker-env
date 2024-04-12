// pipeline{
//     agent{
//         kubernetes{
//           defaultContainer 'jnlp'
//             yaml """
// apiVersion: v1
// kind: Pod
// metadata:
//   labels:
//     build-spec: kaniko
// spec:
//   containers:
//     - name: kaniko
//       image: gcr.io/kaniko-project/executor:debug
//       imagePullPolicy: Always
//       command:
//       - '/busybox/cat'
//       tty: true
// //   volumeMounts:
// //     - name: kaniko-secret
// //       mountPath: '/kaniko/.docker'
// // volumes:
// // - name: kaniko-secret
// //   projected:
// //     sources:
// //     - secret:
// //         name: docker-cred
// //         items:
// //           - key: dockerconfigjson
// //             path: config.json
// """
//         }
//     }
//     stages  {
//         stage('build with kaniko'){
//           environment{
//            DOCKER_USERNAME = credentials('docker-credentials').username
//            DOCKER_PASSWORD = credentials('docker-credentials').password
//             // KANIKO_DOCKER_CREDS=credentials('docker-credentials')
//           }
//             steps {
//                 container('kaniko'){
//                    sh "echo '{\"auths\":{\"docker.io\":{\"username\":\"$DOCKER_USERNAME\",\"password\":\"$DOCKER_PASSWORD\",\"email\":\"\"}}}' > /kaniko/.docker/config.json"
//                    sh "/kaniko/executor -f `pwd`/Dockerfile -c `pwd` --destination=gowtham014/docker-env:1.0"
//                     }
//                 }
//         }
//     }
// }

pipeline {
    agent {
        kubernetes {
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
"""
        }
    }
    stages {
        stage('Build with Kaniko') {
            environment {
                DOCKER_CONFIG = '/kaniko/.docker'
            }
            steps {
                container('kaniko') {
                    withCredentials([usernamePassword(credentialsId: 'docker-credentials', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                        sh "mkdir -p $DOCKER_CONFIG"
                        sh "echo '{\"auths\":{\"docker.io\":{\"username\":\"$DOCKER_USERNAME\",\"password\":\"$DOCKER_PASSWORD\",\"email\":\"\"}}}' > $DOCKER_CONFIG/config.json"
                        sh "/kaniko/executor -f `pwd`/Dockerfile -c `pwd` --destination=gowtham014/docker-env:1.0"
                    }
                }
            }
        }
    }
}

