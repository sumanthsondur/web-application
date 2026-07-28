pipeline {
    agent any
    
    tools {
        jdk 'java-11'
        maven 'maven'
    }
    
    stages {
        stage('Git-checkout') {
            steps {
                git branch: 'dev', url: 'https://github.com/manjukolkar/web-application.git'
            }
        }
        
        stage('Code Compile') {
            steps {
                sh 'mvn compile'
            }
        }
        
        stage('Code Package') {
            steps {
                sh 'mvn clean install'
            }
        }
        
        stage('Build and tag') {
            steps {
                sh 'docker build -t sumanthsondur/project-1 .'
            }
        }
        
        stage('Containerisation') {
            steps {
                // Removed -it to prevent the build runner from freezing
                sh 'docker run -d --name c8 -p 9008:8080 sumanthsondur/project-1'
            }
        }
        
        stage('Push to Docker Hub') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                        // Notice both login and push happen inside this single string block
                        sh '''
                            echo "$DOCKER_PASSWORD" | docker login -u "$DOCKER_USERNAME" --password-stdin
                            docker push sumanthsondur/project-1
                        '''
                    }
                }
            }
        }
    }
}
