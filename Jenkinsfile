// Uses Declarative syntax to run commands inside a container.
pipeline {
    agent {
        kubernetes {
            // Rather than inline YAML, in a multibranch Pipeline you could use: yamlFile 'jenkins-pod.yaml'
            // Or, to avoid YAML:
            // containerTemplate {
            //     name 'shell'
            //     image 'ubuntu'
            //     command 'sleep'
            //     args 'infinity'
            // }
            // Can also wrap individual steps:
            // container('shell') {
            //     sh 'hostname'
            // }
            yamlFile 'KubernetesPod.yaml'
            defaultContainer 'nodejs'
            retries 2
        }
    }
    stages {
        stage('Test npm') {
            steps {
                container('nodejs'){
                    sh 'npm -v'
                }
                
            }
        }
        stage('test Kaniko') {
            steps {
                container('kaniko') {
                    sh """
                    /kaniko/executor \
                    --dockerfile=Dockerfile \
                    --context=`pwd` \
                    --destination=docker.io/anas1243/solar-app:latest
                    """
                }
            }
        }
    }
}
