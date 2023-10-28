
pipeline {
    agent none
    stages {
        stage('Build') {
            agent {
                label 'Node 1'
            }
            steps {
                // Define build steps here
                sh 'make'
            }
        }
        stage('Test') {
            agent {
                label 'Node 1'
            }
            steps {
                // Define test steps here
                sh 'make test'
            }
        }
        stage('Deploy') {
            agent {
                label 'Node '
            }
            steps {
                // Define deployment steps here
                sh 'make deploy'
            }
        }
    }
    post {
        success {
            echo 'Pipeline succeeded! Send success notification.'
            // Additional success actions
        }
        failure {
            echo 'Pipeline failed! Send failure notification.'
            // Additional failure actions
        }
    }
}
