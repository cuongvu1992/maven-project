pipeline {
    agent any

    tools{
        maven 'maven 3.3.9'
    }

    parameters {
         string(name: 'slave_machine', defaultValue: '172.31.48.41', description: 'Jenkins Slave')
    }

    triggers {
        pollSCM('* * * * *')
    }
    stages{
        stage('Build'){
            steps {
                sh 'mvn clean package'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }

        stage('Deployments') {
            steps {
                sh "cd /opt/apache-tomcat-8.5.31/"
                sh "scp -v -o StrictHostKeyChecking=no -i /var/lib/jenkins/jenkins-user **/target/*.war jenkins-user@${params.slave_machine}:/var/lib/tomcat8/webapps"
            }
        }
    }
}
