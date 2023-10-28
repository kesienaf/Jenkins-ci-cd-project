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
        success {
            script {
                def buildNumber = currentBuild.number
                def buildStatus = currentBuild.result
                def jobName = env.JOB_NAME
                
                emailext subject: "Deployment Successful - Build #${buildNumber} - ${buildStatus}",
                          body: """
                                Job Name: ${jobName}
                                Build Number: ${buildNumber}
                                Build Status: ${buildStatus}
                                
                                Console Output:
                                ${Jenkins.instance.getItemByFullName(jobName).getBuildByNumber(buildNumber).log}
                              """,
                          to: 'kesienafels@gmail.com'
            }
        }
        failure {
            script {
                def buildNumber = currentBuild.number
                def buildStatus = currentBuild.result
                def jobName = env.JOB_NAME
                
                emailext subject: "Deployment Failed - Build #${buildNumber} - ${buildStatus}",
                          body: """
                                Job Name: ${jobName}
                                Build Number: ${buildNumber}
                                Build Status: ${buildStatus}
                                
                                Console Output:
                                ${Jenkins.instance.getItemByFullName(jobName).getBuildByNumber(buildNumber).log}
                              """,
                          to: 'kesienafels@gmail.com'
        }
    }
}
