pipeline {
    agent none
    parameters {
        string(name: 'IMAGE', defaultValue: 'isbee/spinnaker-test:2.0', description: 'Spinnaker with Jenkins')
    }
    stages {
        stage('Build docker image') {
            agent any
            steps {
                sh "docker build -t ${params.IMAGE} ."
            }
        }
        stage('Push docker image') {
            agent any
            steps {
				sh "docker login -u \"isbee\" -p \"dltmdgus2!\" docker.io"
                sh "docker push ${params.IMAGE}"
            }
        }
    }
}

