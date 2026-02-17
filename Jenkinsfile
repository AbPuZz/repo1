pipeline {
    agent any

    environment {
        SFTP_USER = 'isma'
        SFTP_PASS = '1234'
        SFTP_DIR  = './data'
        SFTP_PORT = '2222'
        IMAGE     = 'atmoz/sftp'
        CONTAINER = 'sftp_test'
    }

    stages {
        stage('Preparar') {
            steps {
                // Asegúrate de que Docker está disponible
                sh 'docker --version'
            }
        }

        stage('Levantar contenedor SFTP') {
            steps {
                sh """
                docker rm -f $CONTAINER || true
                docker run -d --name $CONTAINER -p $SFTP_PORT:22 -v $SFTP_DIR:/home/$SFTP_USER/upload $IMAGE $SFTP_USER:$SFTP_PASS:::upload
                """
            }
        }

        stage('Probar conexión SFTP') {
            steps {
                // Instala lftp o usa sftp CLI para probar (opcional)
                sh """
                echo "put prueba.txt" | sftp -oPort=$SFTP_PORT $SFTP_USER@localhost
                """
            }
        }

        stage('Detener contenedor') {
            steps {
                sh "docker stop $CONTAINER"
            }
        }
    }

    post {
        always {
            echo 'Pipeline terminado'
            sh "docker rm -f $CONTAINER || true"
        }
    }
}
