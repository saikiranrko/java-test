pipeline {
    agent any

    environment {
        function_name = 'java_sample'
    }

    stages {
        stage('Build') {
            steps {
                echo 'Build'
                sh 'mvn package'
            }
        }

        stage('Push') {
            steps {
                echo 'Push'

                sh "aws s3 cp target/sample-1.0.3.jar s3://bucket32011"
            }
        }
        stage('SonarQube') {
            steps {
                echo 'Sonar-scanner'
                //sh 'mvn package'
            }
        }

        stage('Deploy') {
            steps {
                echo 'deploy'

                sh "aws lambda update-function-code --function-name $function_name --region us-east-1 --s3-bucket bucket32011 --s3-key sample-1.0.3.jar"
            }
        }
    }
}

