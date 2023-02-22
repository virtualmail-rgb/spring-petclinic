pipeline{
    agent any
    stages{
        stage('Git clone'){
            steps{
                git branch: 'main', url: 'https://github.com/virtualmail-rgb/spring-petclinic.git'  
                mail subject: 'Git clone is successfull',
                        body: 'Clonning is successful',
                        to  : 'virtualdevops@gmail.com'
            }
        }
        stage('SonarQube'){
            steps{
                withSonarQubeEnv('SONAR-QUBE'){
                    sh 'mvn clean install sonar:sonar'
                }
                mail subject: 'Codescan is successfull',
                        body: 'static code analysis is successful',
                        to  : 'virtualdevops@gmail.com'
            }
        } 
        stage('artifactory'){
            steps{
                rtMavenDeployer (
                    id: "MAVEN_DEPLOYER",
                    serverId: 'jfrog_instance',
                    releaseRepo: 'qtdevops-libs-release-local',
                    snapshotRepo: 'qtdevops-libs-snapshot-local'
                )
            }
        }
        stage('Publishtheartifacts'){
            steps{
                rtPublishBuildInfo (
                    serverId: 'jfrog_instance'
                )
            }
        }
        stage('junits test reports'){
            steps{
                junit testResults: 'target/surefire-reports/*.xml'
            }
        }
    }
    post {
            always {
                echo "job done or build copmleted"
                mail subject: 'Build completed for Jenkins JOB $env.JOB_NAME',
                    body: 'for Jenkins JOB $env.JOB_NAME \n Click Here: $env.JOB_URL',
                    to: "virtualdevops@gmail.com"
            }
            success {
                echo "Build is successfull"
                mail subject: 'for Jenkins JOB $env.JOB_NAME with Build ID $env.BUILD_ID',
                    body: 'for Jenkins JOB $env.JOB_NAME',
                    to: "virtualdevops@gmail.com"
            }
            failure {
                echo "Build is failed"
                mail subject: 'Build is failed Jenkins JOB $env.JOB_NAME with Build ID $env.BUILD_ID',
                    body: 'Jenkins job is failed JOB $env.JOB_NAME',
                    to: "virtualdevops@gmail.com"
            }
    }
}