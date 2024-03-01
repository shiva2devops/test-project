REGISTRY_CREDENTIALS = credentials('docker-creds')
DOCKER_IMAGE = "mshiva7396/test-project:${BUILD_NUMBER}"
pipeline {
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
                    sh 'mvn clean package -DskipTests=true'
                }
            }
        }
        
        stage('Test') {
            steps {
                script {
                    sh 'mvn test'
                }
            }
        }
        
        stage('Docker Build') {
            steps {
                script {
                   sh 'docker build -t ${DOCKER_IMAGE} .'
                }
            }
        }
        
        // stage('Docker Push') {
        //     steps {
        //         script {
        //             docker.withRegistry('https://your.registry.url', 'docker-credentials-id') {
        //                 docker.image('your-docker-image-name:latest').push('latest')
        //             }
        //         }
        //     }
        // }
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
