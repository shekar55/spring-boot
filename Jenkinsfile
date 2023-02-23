pipeline {
    agent any 
      tools {
    	maven "mvn363"
        gradle "g6.3"
    } 
       
      stages {
        
        stage('Clone Repo') {
          steps {
            sh 'rm -rf spring-boot'
            sh 'git clone https://github.com/shekar55/spring-boot.git'
            }
        }
        
        stage('Build'){
		steps{
			sh "./gradlew test"
		}
	  }
       
        stage('Build Docker Image') {
            steps {
              sh 'docker build -t chandu5562/springboot:latest .'
              }
        }
        stage('Push Docker image') {
            environment {
                DOCKER_HUB_LOGIN = credentials('docker')
            }
            steps {
                sh 'docker login --username=$DOCKER_HUB_LOGIN_USR --password=$DOCKER_HUB_LOGIN_PSW'
                sh    'docker push chandu5562/springboot:latest'
            }
        }
          
        stage('K8S Deploy') {
            steps {
                withKubeConfig([credentialsId: 'k8s', serverUrl: '']) {
                    sh ('kubectl apply -f  kube.yaml')
                }
            }
        }
        stage('Check WebApp Rechability') {
          steps {
          sh 'sleep 10s'
          sh ' curl http://3.108.67.219:30007'
          }
        }
        
      }
}
