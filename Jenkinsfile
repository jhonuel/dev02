pipeline {
    agent { label 'docker-agent' }

    environment {
        GITHUB_REPO = "https://github.com/jhonuel/dev02.git"
        GIT_BRANCH = "main"
    }

    stages {
        stage('Clone Repository') {
            steps {
                echo 'Clonando el repositorio...'
                sh 'git --version' // Verificar si git está instalado
                sh 'rm -rf dev || true' // Asegurar que no existe un directorio previo
                sh 'git clone ${GITHUB_REPO} -b ${GIT_BRANCH}'
            }
        }

        stage('Build') {
            steps {
                echo 'Compilando el proyecto...'
                // Agrega aquí los comandos específicos para compilar tu proyecto
                sh 'cd dev && mvn clean compile'
            }
        }

        stage('Test') {
            steps {
                echo 'Ejecutando pruebas...'
                // Agrega aquí los comandos específicos para ejecutar las pruebas de tu proyecto
                sh 'cd dev && mvn test'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Desplegando la aplicación...'
                // Detener y eliminar contenedores existentes
                sh 'docker-compose down -v'
                // Construir y levantar los nuevos contenedores
                sh 'docker-compose up -d --build'
            }
        }
    }

    post {
        always {
            echo 'Pipeline finalizado.'
        }
        success {
            echo 'Pipeline completado con éxito.'
        }
        failure {
            echo 'Pipeline fallido.'
        }
    }
}