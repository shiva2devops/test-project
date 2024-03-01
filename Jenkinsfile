
pipeline {
    environment { 
        registry = "mshiva7396/test-project" 
        registryCredential = 'docker-creds' 
        dockerImage = '' 
    }
    agent {
        docker {
            image 'maven:latest'
            args '-v /var/run/docker.sock:/var/run/docker.sock'
            args '-u root'
        }
    }
    
    stages {
        stage('Build') {
            steps {
                script {
                    sh 'mvn clean deploy -Dmaven.test.skip=true'
                }
            }
        }
        
        stage('Test') {
            steps {
                script {
                    sh 'mvn surefire-report:report'
                }
            }
        }
        
        stage('Docker Build') {
            steps {
                echo 'Building Docker Image'
                script {
                   dockerImage = docker.build registry + ":$BUILD_NUMBER"
                }
            }
        }
        
        stage('Docker Push') {
            steps {
                echo 'Pushing Docker Image'
                script {
                    docker.withRegistry('', registryCredential) {
                        dockerImage.push(${BUILD_NUMBER})
                    }
                }
            }
        }
         stage('clean up'){
            steps{
                sh 'docker rmi $registry:$BUILD_NUMBER'
            }
         }
    }
    
    // post {
    //     success {
    //         echo 'Build and test succeeded. Docker image pushed to registry.'
    //     }
    //     failure {
    //         echo 'Build or test failed. Docker image was not pushed to registry.'
    //     }
    // }
}
