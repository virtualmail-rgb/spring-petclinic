pipeline{
    agents any
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
                    snapshorRepo: 'qtdevops-libs-snapshot-local'
                )
            }
        }
        stage('execmaven'){
            steps{
                rtmavenRun (
                    timeout(time: 1, unit: 'HOURS') {
                waitForQualityGate abortPipeline: true
              }
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