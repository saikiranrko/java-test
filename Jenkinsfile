pipeline {
    agent any

    stages {
        stage('Checkout SCM') {
            steps {
                echo 'Checking out code from SCM (Git)'
                checkout scm
            }
        }

        stage('Build') {
            steps {
                echo 'Building the application'
                sh 'mvn clean package'
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests'
                sh 'mvn test'
            }
        }

        stage('SonarQube') {
            steps {
                echo 'Running SonarQube analysis'
                withSonarQubeEnv('Your_SonarQube_Server_Name') {
                   sh 'mvn sonar:sonar'
                }
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying the application'
                 Your deployment steps go here
            }
        }
    }

    post {
        always {
            echo 'This will always run'
            // Additional post-build actions here
        }
        success {
            echo 'This will run only if the build is successful'
            // Additional actions for successful builds here
        }
        failure {
            echo 'This will run only if the build fails'
            // Additional actions for failed builds here
        }
    }
}
