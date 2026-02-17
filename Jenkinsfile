pipeline {
    agent any
    stages {
        stage('Deploy') {
                steps {
sh '''
rm -rf /var/jenkins_home/web/*
cp -r ./* /var/jenkins_home/web/
'''
            }
        }
    }
    post {
success {
echo 'Web actualizada correctamente'
}
failure {
echo 'Error en el despliegue'
}
}

}