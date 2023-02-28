pipeline{

    agent any

    stages{
        stage("Sonar quality check"){

            agent {
                docker {
                    image 'maven'
                }
            }
            steps{
                script {
                    withSonarQubeEnv(credentialsId: 'sonar-secret') {
                        sh 'mvn clean package sonar:sonar'
                    }
                }
            }
        }
        stage('docker build and docker push to nexus repo'){
            steps{
                script{
                    
                }
            }
        }
        /* stage('Quality gate status'){
            steps{
                script{
                    waitForQualityGate abortPipeline: false, credentialsId: 'sonar-secret'
                }
            }
        }*/
        
    }
}