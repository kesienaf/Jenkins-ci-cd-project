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
                sh "sudo systemctl daemon-reload"
                // sh "~/apache-tomcat-7.0.94/bin/shutdown.sh"
                sh "~/apache-tomcat-7.0.94/bin/startup.sh"
            }
        }
    }
    post {
        failure {
            echo 'Build failed! Email notification will be sent.'
            emailext body: '${SCRIPT, template="groovy-html.template"}',
                      recipientProviders: [[$class: 'CulpritsRecipientProvider']],
                      subject: "Failed: ${currentBuild.fullDisplayName}",
                      to: 'kesienafels@gmail.com'
        }
        
        success {
            echo 'Build successful! Email notification will be sent.'
            emailext body: '${SCRIPT, template="groovy-html.template"}',
                      recipientProviders: [[$class: 'CulpritsRecipientProvider']],
                      subject: "Success: ${currentBuild.fullDisplayName}",
                      to: 'kesienafels@gmail.com'
        }
    }
}

