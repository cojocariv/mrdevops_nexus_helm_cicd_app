pipeline{

    agent any
    environment{
        VERSION = "${env.BUILD_ID}"
    }
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
                    withCredentials([string(credentialsId: 'nexus', variable: 'nexus_creds')]) {
                     sh '''
                     docker build -t 192.168.100.49:8083/springapp:${VERSION} .

                     docker login -u admin -p $nexus_creds 192.168.100.49:8083

                     docker push 192.168.100.49:8083/springapp:${VERSION}

                     docker rmi 192.168.100.49:8083/springapp:${VERSION}

                     '''
                    }
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