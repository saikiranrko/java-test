pipeline {
    agent { label 'master'}

    environment {
        function_name = 'java-sample'
    }

    stages {

        // CI Start

        stage('Declarative: Checkout SCM') {
            steps {
                echo 'Declarative: Checkout SCM'
               // sh 'mvn package'
            }
        } 

        stage('Checkout') {
            steps {
                echo 'Checkout'
                //sh 'mvn package'
            }
        }
        
        stage('Build') {
            steps {
                echo 'Build'
                sh 'mvn package'
            }
        }


        // stage("SonarQube analysis") {
        //     agent any

        //     when {
        //         anyOf {
        //             branch 'feature/*'
        //             branch 'main'
        //         }
        //     }
        //     steps {
        //         withSonarQubeEnv('Sonar') {
        //             sh 'mvn sonar:sonar'
        //         }
        //     }
        // }

        // stage("Quality Gate") {
        //     steps {
        //         script {
        //             try {
        //                 timeout(time: 10, unit: 'MINUTES') {
        //                     waitForQualityGate abortPipeline: true
        //                 }
        //             }
        //             catch (Exception ex) {

        //             }
        //         }
        //     }
        // }

        stage('test') {
            steps {
                echo 'test'

               // sh "aws s3 cp target/sample-1.0.3.jar s3://bucket32011"
            }
        }

        stage('SonarQube') {
            steps {
                echo 'SonarQube'
                //sh 'mvn package'
            }
        }

        // Ci Ended

        // CD Started

        stage('Deployments') {
            parallel {

                stage('Deploy to Dev') {
                    steps {
                        echo 'Build'

                        sh "aws lambda update-function-code --function-name $function_name --region us-east-1 --s3-bucket bucket32011 --s3-key sample-1.0.3.jar"
                    }
                }

                stage('Deploy to test ') {
                    when {
                        branch 'main'
                    }
                    steps {
                        echo 'Build'

                        // sh "aws lambda update-function-code --function-name $function_name --region us-east-1 --s3-bucket bermtecbatch31 --s3-key sample-1.0.3.jar"
                    }
                }
            }
        }

        stage('Deploy to Prod') {
            when {
                branch 'main'
            }
            steps {
               input (
                    message: 'Are we good for Prod Deployment ?'
               )
            }
        }

        stage('Release to Prod') {
            when {
                branch 'main'
            }
            steps {
                sh "aws lambda update-function-code --function-name $function_name --region us-east-1 --s3-bucket bucket32011 --s3-key sample-1.0.3.jar"
            }
        }


        

        // CD Ended
    }

    post {
        always {
            echo "${env.BUILD_ID}"
            echo "${BRANCH_NAME}"
            echo "${BUILD_NUMBER}"

        }

        failure {
            echo 'failed'
        }
        aborted {
            echo 'aborted'
        }
    }
}
