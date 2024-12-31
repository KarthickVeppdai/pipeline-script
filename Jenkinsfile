def readPom
def imageName

pipeline {
    agent any
    
    

environment
{
    PROJECT1_NAME = "springboot-app1"
        DOCKER_REGISTRY = "docker.io"
        DOCKER_IMAGE_NAME = "karthickveppdai/trafffic-app"
        DOCKER_TAG = "latest"
        DOCKER_CREDENTIALS = "d7dc5cc4-7021-4e3d-9f0b-e000bae23e39"

}




    tools {
        // Install the Maven version configured as "M3" and add it to the path.
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
                     imageName = "${readPom.artifactId}:${env.AUTOMATIC_TAG}";
                     }
                     echo "Application Version: ${imageName}"
                
            }
            
        }
    

        stage('Build Docker Image') {
            steps {
                script {
                    // Build Docker image using the Dockerfile in the current directory
                    docker.build("${DOCKER_IMAGE_NAME}:${DOCKER_TAG}")
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
                script {
                    // Push the built image to Docker registry
                    docker.push("${DOCKER_IMAGE_NAME}:${DOCKER_TAG}")
                }
            }
        }



        
    }



}






    

