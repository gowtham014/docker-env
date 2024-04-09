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
                sh '/kaniko/executor -f `pwd`/Dockerfile -c `pwd` '
            }
        }
    }
  }
}