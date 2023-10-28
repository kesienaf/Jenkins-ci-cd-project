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
            }
        }
        stage('Deploy') {
            agent {
                label 'Node 2'
            }
            steps {
                 script {
                    def remoteServer = '172.31.11.28' // Replace with your remote server's IP or hostname
                    def remoteUser = 'centos' // Replace with your remote server's username
                    def pemFilePath = 'kesienaf.pem' // Replace with the path to your .pem file

                    def warFileName = sh(script: "find target -name '*.war' | head -n 1 | xargs -I {} basename {}", returnStdout: true).trim()

                    if (warFileName) {
                        echo "Found .war file: ${warFileName}"
                        sh "scp -i ${kesienaf.pem} target/${warFileName} ${centos}@'172.31.35.225':/home/centos/apache-tomcat-7.0.94/webapps"
                        sh "~/apache-tomcat-7.0.94/bin/shutdown.sh && ~/apache-tomcat-7.0.94/bin/startup.sh"
                    } else {
                        error 'No .war file found in the target directory.'
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
