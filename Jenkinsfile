def readPom
def imageName
def tagName
def userId = "karthickveppdai"
def repository = "trafffic-app"

pipeline {
    agent any
environment
{
        PROJECT1_NAME = "springboot-app1"
        DOCKER_REGISTRY = "docker.io"
        DOCKER_IMAGE_NAME = "trafffic-app"
        DOCKER_TAG = "latest"
        DOCKER_CREDENTIALS = "d7dc5cc4-7021-4e3d-9f0b-e000bae23e39"
}

    tools {      
        maven "Maven 3.6.3"
        dockerTool 'docker'
    }

    stages {
        stage('Build') {
            
            steps {
               
                git branch: 'main', credentialsId: 'b3217732-06ce-407e-a974-a6af3f476792', url: 'https://github.com/KarthickVeppdai/traffic.git'
                sh "mvn clean install"
                      echo "Application is running with Jenkins Build Number #${env.BUILD_ID}"
                     script {
                     readPom = readMavenPom file: 'pom.xml';                   
                     }
                     echo "Application Version: ${imageName}"
                
            }
            
        }
    

        stage('Build Docker Image') {
            steps {
                script {
                    // Build Docker image using the Dockerfile in the current directory
                    docker.build("${repository}:${env.BUILD_ID}")
                }
            }
        }

        stage('Login to Docker Registry') {
            steps {
                script {
                    // Log in to Docker registry using stored Jenkins credentials
                    withCredentials([usernamePassword(credentialsId: "${DOCKER_CREDENTIALS}", usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                        sh 'echo $DOCKER_PASSWORD | docker login -u $DOCKER_USERNAME --password-stdin'
                    }
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                
                    sh "docker tag ${repository} ${repository}:${env.BUILD_ID}"
                    sh "docker push ${repository}:${env.BUILD_ID}"
                
            }
        }



        
    }



}






    

