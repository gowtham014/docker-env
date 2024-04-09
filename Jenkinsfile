// pipeline{
//     kubernetes{
//               yaml """
// kind: Pod
// metadata:
//   name: kaniko
// spec:
//   containers:
//   - name: jnlp
//     workingDir: /home/jenkins
//   - name: kaniko
//     workingDir: /home/jenkins
//     image: gcr.io/kaniko-project/executor:debug
//     imagePullPolicy: Always
//     command:
//     - /busybox/cat
//     tty: true 
//     """
//     }
//     stages{
//         stage('build with kaniko'){
//             container('kaniko'){
//                 sh '/kaniko/executor -f `pwd`/Dockerfile -c `pwd` '
//             }
//         }
//     }
// }

pipeline{
  agent{
    kubernetes{
      yaml"""
      apiVersion: v1
      kind: Pod
      metadata:
        labels:
          build-spec: kaniko
        namespace: jenkins
      spec:
        containers:
          - name: kaniko
            image: gcr.io/kaniko-project/executor:debug
            imagePullPolicy: Always
            command:
            - /busybox/cat
            tty: true
            """
    }
  }
  stages{
        stage('build with kaniko'){
          steps{
            container('kaniko'){
              withCredentials([usernamePassword(credentialsId: 'docker-credentials', passwordVariable: 'DOCKER_PASSWORD', usernameVariable: 'DOCKER_USERNAME')]) {
              sh "echo $DOCKER_PASSWORD | docker login -u $DOCKER_USERNAME --password-stdin gowtham014"
              sh '/kaniko/executor -f `pwd`/Dockerfile -c `pwd` --destination=gowtham014/docker-dev:1.0 '
            }
        }
    }
  }
}
}

// pipeline {
//     agent {
//         kubernetes {
//             label 'kaniko'
//             defaultContainer 'kaniko'
//             yaml """
// apiVersion: v1
// kind: Pod
// metadata:
//   labels:
//     some-label: some-label-value
// spec:
//   containers:
//   - name: kaniko
//     image: gcr.io/kaniko-project/executor:latest
//     args:
//     - --dockerfile=Dockerfile
//     - --destination=gowtham014/docker-env:latest
//     securityContext:
//       privileged: false
// """
//         }
//     }
//     stages {
//         stage('Build') {
//             steps {
//                 container('kaniko') {
//                     withCredentials([usernamePassword(credentialsId: 'docker-credentials', passwordVariable: 'DOCKER_PASSWORD', usernameVariable: 'DOCKER_USERNAME')]) {
//                         sh "echo $DOCKER_PASSWORD | docker login -u $DOCKER_USERNAME --password-stdin your-registry"
//                         sh 'echo "Building Docker image using Kaniko"'
//                     }
//                 }
//             }
//         }
//     }
// }
