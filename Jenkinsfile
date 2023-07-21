pipeline {
    agent any

    parameters {
        string(name: 'GIT_BRANCH', defaultValue: 'master', description: 'Git branch to build')
        booleanParam(name: 'RUN_TESTS', defaultValue: true, description: 'Run tests during the build')
    }

    environment {
        // Reading default Jenkins environment variables
        def defaultNode = env.NODE_NAME
        def defaultWorkspace = env.WORKSPACE
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out code from Git'
                checkout([$class: 'GitSCM', branches: [[name: "${params.GIT_BRANCH}"]], userRemoteConfigs: [[url: 'https://github.com/your-repo.git']]])
            }
        }

        stage('Build') {
            steps {
                echo "Building on ${defaultNode} using ${defaultWorkspace}"
                sh 'mvn clean package'
            }
        }

        stage('Test') {
            when {
                expression { params.RUN_TESTS }
            }
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
                // Your deployment steps go here
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
