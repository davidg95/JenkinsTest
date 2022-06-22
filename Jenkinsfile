pipeline {
    agent any

    stages {
        stage('Prepare Build') {
            steps {
                echo 'Preparing Build...'
                bat 'git clean -fdx'
                bat 'echo -${BUILD_NUMBER} >> version.txt'
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
                bat 'ant -buildfile deploy/deploy.xml deploy-application'
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
                bat 'sleep 10'
            }
        }
    }
}