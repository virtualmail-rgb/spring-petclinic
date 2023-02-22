pipeline{
    agent any
    stages{
        stage('Git clone'){
            steps{
                git branch: 'main', url: 'https://github.com/virtualmail-rgb/spring-petclinic.git'
            }
        }
        stage('SonarQube'){
            steps{
                withSonarQubeEnv('SONAR-QUBE'){
                    sh 'mvn clean install sonar:sonar'
                }
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
        stage('execmaven'){
            steps{
                rtmavenRun (
                    tool: 'MVN',
                    pom: 'pom.xml',
                    goals: 'deploy',
                    deployerId: "MAVEN_DEPLOYER"
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
    }
}