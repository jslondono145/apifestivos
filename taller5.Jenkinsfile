pipeline {
    agent any

    environment {
        API1_NAME = 'api-fechas'
        API2_NAME = 'api-festivos'
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/jslondono145/apifestivos.git'
            }
        }

        stage('Build Docker Images') {
            steps {
                script {
                    docker.build("${API1_NAME}", "ApiFechas")
                    docker.build("${API2_NAME}", "ApiFestivos")
                }
            }
        }

        stage('Run Containers') {
            steps {
                script {
                    sh 'docker stop api-fechas || true'
                    sh 'docker stop api-festivos || true'
                    sh 'docker rm api-fechas || true'
                    sh 'docker rm api-festivos || true'

                    sh 'docker run -d --name api-fechas -p 5001:80 api-fechas'
                    sh 'docker run -d --name api-festivos -p 5002:80 api-festivos'
                }
            }
        }
    }

    post {
        success {
            echo 'Despliegue exitoso'
        }
        failure {
            echo 'El pipeline falló'
        }
    }
}
