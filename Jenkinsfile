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
    post {
		always {
			mail bcc: '', body: "<br>Project: ${env.JOB_NAME} <br>Build Number: ${env.BUILD_NUMBER} <br> URL de build: ${env.BUILD_URL}", cc: '', charset: 'UTF-8', from: '', mimeType: 'text/html', replyTo: '', subject: "${currentBuild.result} CI: Project name -> ${env.JOB_NAME}", to: "cojocari.v@gmail.com";  
		}
	}
}