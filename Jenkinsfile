pipeline {
    agent { label 'JDK-17' }
    options {
        timeout(time: 30, unit: 'MINUTES')
    }
    triggers {
        pollSCM('* * * * *')
    }
    tools {
        jdk 'JDK_17'
    }
    stages {
        stage('vcs') {
            steps {
                git url: 'https://github.com/dummyrepos/spring-petclinic-1.git',
                    branch: 'develop'
            }
        }
                                
    }
          
        stage('build and package') {
            steps {
                rtMavenDeployer (
                    id: "SPC_DEPLOYER",
                    serverId: "jfrog_jenkins",
                    releaseRepo: 'jenkins-libs-release',
                    snapshotRepo: 'jenkins-libs-snapshot'
                )
            }        
        }    
        
        stage('execute maven') {
             steps {
                rtMavenRun (
                    tool: 'maven-3.9.3'
                    pom:'pom.xml',
                    goals: 'clean install',
                    deployerId: "SPC_DEPLOYER"
                )
            } 
        }
        stage('Publish build info') {
            steps {
                rtPublishBuildInfo (
                    serverId: "jfrog_jenkins"
                )
            } 
        }
        stage('SonarQube analysis') {
            steps {
                withSonarQubeEnv('SONARCLOUD') {
                          sh 'mvn clean package sonar:sonar -Dsonar.organization=spring-application -Dsonar.token=a49b9d99ab452ad93290f7e7337eee4f23507d07 -Dsonar.projectKey=spring-application_sonar-spring'
                }
            }    
        }
        stage('results') {
            steps{
                archiveArtifacts artifacts: '**/target/*.jar'
                junit testresults: '**/target/surefire-reports/*.xml'
            }
        }

    // post {
    //     sucess {
    //         mail subject: 'this project is sucess',
    //              body: 'this spc project is success',
    //              to: 'rahulmaddy14@gmail.com'

    //     }
    //     failure{
    //         mail subject: 'this project is failure',
    //              body: 'this project has failed',
    //              to: 'rahulmaddy14@gmail.com'

    //     }
        
    // }
}        
