pipeline {
    agent { label 'NODE_1' }
    triggers {
        pollSCM('* * * * *')
    }
    stages {
        stage("mail"){
            steps{
                mail subject: 'Build Started', 
                body: 'Build started',
                to: 'virtualdevops456@gmail.com'
                git branch: 'main', url: 'https://github.com/virtualmail-rgb/spring-petclinic.git'
            }
        }
        stage("build"){
            steps{
                sh 'mvn package'
            }
        }
        stage("archive results"){
            steps{
                junit '**/surefire-reports/*.xml'
            }
        }
    }
    post {
        always {
            echo 'Job completed'
            mail subject: 'Build Completed', 
                  body: 'Build Completed', 
                  to: 'qtdevops@gmail.com'
        }
        failure {
            mail subject: 'Build Failed', 
                  body: 'Build Failed', 
                  to: 'qtdevops@gmail.com' 
        }
        success {
            junit '**/surefire-reports/*.xml'
        }
    }
}
