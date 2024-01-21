pipeline {
    agent any

    stages {
        stage('Checkout from GitHub') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/psivaramps/nodeapp-jenkins-docker.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                //sh 'docker build -t my-node-js:0.1 .'
                script {
                    dockerImage = docker.build("spamarthy/my-nodejs-app:${env.BUILD_NUMBER}")
                    
                }
            }
        }

        stage('Push Docker Image to Docker Hub') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'spamarthy') {
                        dockerImage.push()
                    }
                }
            }
        }


        stage('Run Container from Docker Hub') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'spamarthy') {  
                    docker.image("spamarthy/my-nodejs-app:${env.BUILD_NUMBER}").run('-p 5000:5000 -d')
            }
        }
    }
}
          stage('Check Application with curl') {
            steps {
                script {
                    //sh "curl -f http://192.168.208.128/:5000"
                    sh "curl -f http://localhost:5000"  // Replace PORT and ENDPOINT
                        }
                    }
            }
    }
}

