pipeline{
    agent{
        kubernetes{
            yaml """
            apiVersion: v1
            kind: Pod
            spec:
            containers:
            - name: kaniko
                image: gcr.io/kaniko-project/executor:debug
                command:
                - sleep
                args:
                - 9999999
                volumeMounts:
                - name: kaniko-secret
                mountPath: /kaniko/.docker
            restartPolicy: Never
            volumes:
            - name: kaniko-secret
                secret:
                    secretName: docker-cred
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