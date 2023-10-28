pipeline {
    agent none
    stages {
        stage('Build') {
            agent {
                label 'Node 1'
            }
            steps {
                echo 'Building the application'
                // Define build steps here
                sh '/opt/maven/bin/mvn clean package'
            }
        }
        stage('Test') {
            agent {
                label 'Node 1'
            }
            steps {
                echo 'Running tests'
                // Define test steps here
                sh 'mvn test'
                stash (name: 'JenkinsProject', includes: "target/*.war")
            }
        }
        stage('Deploy') {
            agent {
                label 'Node 2'
            }
            steps {
                echo 'Deploying the application'
                // Define deployment steps here
                unstash 'JenkinsProject'
                sh "sudo rm -rf ~/apache*/webapp/*.war" 
                sh "sudo mv target/*.war ~/apache*/webapps/"
                sh "~/apache-tomcat-7.0.94/bin/shutdown.sh && ~/apache-tomcat-7.0.94/bin/startup.sh"
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
