pipeline{
    agent any
    stages{
        stage('Git clone'){
            steps{
                git branch: 'main', url: 'https://github.com/virtualmail-rgb/spring-petclinic.git'  
            }
            post {
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
            }
            post {
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
    post{
            always{
                echo "job done or build copmleted"
                mail subject: 'Build completed'
                    body: 'build status'
                    to: "virtualdevops@gmail.com"
            }
            success{
                echo "Build is successfull"
                mail subject: 'Build completed'
                    body: 'build status'
                    to: "virtualdevops@gmail.com"
            }
            failure{
                echo "Build is failed"
                mail subject: 'Build is failed'
                    body: 'build status'
                    to: "virtualdevops@gmail.com"
            }
        }
}