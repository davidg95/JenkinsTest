pipeline {
    agent {
        label 'MASTER'
    }

    stages {
        stage('Prepare Build') {
            steps {
                echo 'Preparing Build...'
                bat 'git clean -fdx'
                bat 'echo -${BUILD_NUMBER} >> version.txt'
                echo 'Agent Name: ${env.NODE_NAME}'
            }
        }
        stage('Build') {
            steps {
                echo 'Building...'
                bat 'gradle clean build'
            }
        }
        stage('Deploy') {
            when {
                expression {
                    currentBuild.result == null || currentBuild.result == 'SUCCESS'
                }
            }
            steps {
                echo 'Deploying application...'
                node('HOST') {
                    bat 'ant -buildfile deploy/deploy.xml deploy-application'
                }
            }
        }
        stage('Execute') {
            when {
                expression {
                    currentBuild.result == null || currentBuild.result == 'SUCCESS'
                }
            }
            steps {
                echo 'Executing application...'
                node('HOST') {
                    bat 'java -jar C:/Jenkins-Test/JenkinsTest.jar'
                }
            }
        }
    }
}