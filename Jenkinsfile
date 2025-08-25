pipeline {
    agent {
        label 'AGENT-1'
    }

    options {
        timeout(time: 30, unit: 'MINUTES')
        disableConcurrentBuilds()
    }

    stages {
        stage('Build') {
            steps {
                script{
                    sh """
                        echo "Hello world!"
                        sleep 10
                        env
                    """
                }
            }
        }
        stage('Test') {
            steps {
                echo 'Testing..'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
            }
        }
    }
}