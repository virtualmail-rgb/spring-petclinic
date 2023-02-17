pipeline {
    agent any
    stages {
        stage ('git') {
            steps {
                git branch: 'main', url: 'https://github.com/virtualmail-rgb/spring-petclinic.git'
            }
        }
        stage ('Artifactory Configuration'){
            steps {
                rtMavenDeployer (
                    id: 'MAVEN_DEPLOYER',
                    serverId: 'ARTIFACTORY_SERVER',
                    releaseRepo: 'qtdevops-libs-release-local',
                    snapshotRepo: 'qtdevops-libs-snapshot-local'
                )

            }
        }
        stage('Execute Maven'){
            steps {
                rtMavenRun (
                    tool: 'MVN',
                    pom: 'pom.xml',
                    goals: 'clean Install',
                    deployerId: "MAVEN_DEPLOYER"
                )
            }
        }
        stage('Publish Build info'){
            steps{
                rtPublishBuildinfo(
                    serverId:"jfrog_instance"
                )
            }
        }
    }
}