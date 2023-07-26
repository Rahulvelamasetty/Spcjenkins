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

        stage('results') {
            steps{
                archiveArtifacts artifacts: '**/target/*.jar'
                junit testresults: '**/target/surefire-reports/*.xml'
            }
        }

    }
    post {
        sucess {
            mail subject: 'this project is sucess',
                 body: 'this spc project is success',
                 to: 'rahulmaddy14@gmail.com'

        }
        failure{
            mail subject: 'this project is failure',
                 body: 'this project has failed',
                 to: 'rahulmaddy14@gmail.com'

        }