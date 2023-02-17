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
                    serverId: 'jfrog_instance',
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
                    goals: 'clean install',
                    deployerId: "MAVEN_DEPLOYER"
                )
            }
        }
        stage('publish'){
            steps{
                rtPublishBuildInfo (
                    serverId: "jfrog_instance"
                )
            }
        }
        stage('junit test reports'){
            steps {
                junit testResults: 'target/surefire-reports/*.xml'
            }
        }
    }
}