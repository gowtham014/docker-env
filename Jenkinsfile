pipeline{
    kubernetes{
              yaml """
kind: Pod
metadata:
  name: kaniko
spec:
  containers:
  - name: jnlp
    workingDir: /home/jenkins
  - name: kaniko
    workingDir: /home/jenkins
    image: gcr.io/kaniko-project/executor:debug
    imagePullPolicy: Always
    command:
    - /busybox/cat
    tty: true 
    """
    }
    stages{
        stage('build with kaniko'){
            container('kaniko'){
                sh '/kaniko/executor -f `pwd`/Dockerfile -c `pwd` '
            }
        }
    }
}