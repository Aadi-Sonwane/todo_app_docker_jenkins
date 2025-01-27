@Library('Shared') _
pipeline {
    agent { label 'vinod' }

    stages {
        stage('first') {
            steps {
                script{
                   first()
                    echo "We are testing the code"
                }
            }
        }

        stage('Code') {
            steps {
                script{
                clone("https://github.com/Aadi-Sonwane/todo_app_docker_jenkins.git","main")
                }
            }
        }

        stage('Build') {
            steps {
                script{
                    docker_build("todo_app","latest","aadus")
                }
            }
        }
        
    
        stage('Push to DockerHub') {
            steps {
               script{
                   docker_push("todo_app","latest","aadus")
               }
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying the Docker container'
                sh "docker compose down"
            }
        }

    }
}
