pipeline {
    agent any

    stages {
        stage('Prepare Build') {
            steps {
                echo 'Preparing Build...'
                sh 'git clean -fdx'
                sh 'echo -${BUILD_NUMBER} >> version.txt'
            }
        }
        stage('Build') {
            steps {
                echo 'Building...'
                sh 'gradle clean build'
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
                sh 'ant -buildfile deploy/deploy.xml deploy-application'
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
                sh 'sleep 10'
            }
        }
    }
}