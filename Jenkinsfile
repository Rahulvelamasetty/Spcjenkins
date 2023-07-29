pipeline {
    agent { label 'JDK-17' }
    
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
                git url: 'https://github.com/Rahulvelamasetty/Spcjenkins.git',
                    branch: 'develop'
            }
        }
         stage('build and package') {
            steps {
                rtMavenDeployer (
                    id: "SPC_DEPLOYER",
                    serverId: "JFROG_CLOUD",
                    releaseRepo: 'spc-libs-release',
                    snapshotRepo: 'spc-libs-snapshot'
                )
            }        
        }    
        
        stage('execute maven') {
            steps {
                rtMavenRun (
                    tool: 'maven-3.9.3',
                    pom:'pom.xml',
                    goals: 'clean install',
                    deployerId: "SPC_DEPLOYER"
                )
            } 
        }
        stage('Publish build info') {
            steps {
                rtPublishBuildInfo (
                    serverId: "JFROG_CLOUD"
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
                archiveArtifacts artifacts: '**/target/*.jar', followSymlinks: false
                junit '**/target/surefire-reports/*.xml'
            }
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

