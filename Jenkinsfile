/*pipeline {
    stages {
        stage('Build Image') {
            script {
                env.TEMP = sh(returnStdout: true, script: 'docker images --format "json" | grep "myflaskapp" | grep "Tag"').trim()
                env.TAG = readJSON(text: {env.TEMP}).Tag
                env.NEWTAG = {env.TAG}
            }
            steps {
                sh 'docker images --format "json" | grep "myflaskapp" | grep "Tag"'
            }
        }
    }
}*/

pipeline {
    agent any

    environment {
       TEMP = sh (
            script: "docker ps --filter 'expose=5000' --quiet",
            returnStatus: true
        ).trim()
    }

    stages {
        stage('Checkout Code') {
                steps {
                checkout scmGit(
                    branches: [[name: 'master']],
                    userRemoteConfigs: [[credentialsId: 'dbce9fc0-ae78-442f-b238-461816314ae3',
                    url: 'https://github.com/mrdav/MyFlaskApp.git']])
                }
}
        stage('Build Docker Image') {
            steps {
                dir('/home/ubuntu/docker-projects/Jenkins/'){
                script {
                    // Run the Docker container
                    sh 'docker build . -t myflaskapp:0.1'
                }
                }
                // Confirming that the image is built
                sh 'docker images | grep myflaskapp'
            }
        }
                stage('Stop Previous Container'){


                steps{
                    script {
                        if (env.TEMP != null) {
                            sh 'docker stop ${TEMP}'
                        } else {
                            echo 'Port 5000 is not used'
                    }
            }
        }
                }
        stage('Run Docker Container') {
            steps {
                    // Run the Docker container
                    sh 'docker run -d -p 80:5000 myflaskapp:0.1'

                // Confirming that the container is running
                       sh 'docker ps'
            }
        }
        }
    } 
