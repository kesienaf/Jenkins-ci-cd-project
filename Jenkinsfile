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
                sh "~/apache-tomcat-7.0.94/bin/startup.sh"
                sh "sudo rm -rf ~/apache*/webapp/*.war" 
                sh "sudo mv target/*.war ~/apache*/webapps/"
                sh "systemctl daemon-reload"
                // sh "~/apache-tomcat-7.0.94/bin/shutdown.sh"
                sh "~/apache-tomcat-7.0.94/bin/startup.sh"
            }
        }
    }
    post {
        success {
            emailext subject: 'Deployment Successful',
                      body: 'The deployment was successful. Please verify the application.',
                      to: 'kesienafels@gmail.com',
                      from: 'jenkins@example.com'
        }
        failure {
            emailext subject: 'Deployment Failed',
                      body: 'The deployment failed. Please check the Jenkins build logs for more information.',
                      to: 'kesienafels@gmail.com',
                      from: 'jenkins@example.com'
        }
    }
}
