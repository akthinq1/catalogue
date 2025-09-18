pipeline {
    agent {
        label 'AGENT-1'
    }

    environment{
        appVersion = ""
        REGION = "us-east-1"
        ACC_ID = "741005748171"
        PROJECT = "roboshop"
        COMPONENT = "catalogue"
    }

    options {
        timeout(time: 30, unit: 'MINUTES')
        disableConcurrentBuilds()
    }

    parameters{
        // choice(name:'deploy_to', choice: 'DEPLOY', description: 'Pick the env')
        booleanParam(name: 'DEPLOY', defaultValue: false, description: "Trigger the value")
    }

    // Build
    stages {
        
        // read version
        stage('Read package.json'){
            steps{
                script{
                    def packageJson = readJSON file: 'package.json'
                    appVersion = packageJson.version
                    echo "Package version: ${appVersion}"
                }
            }
        }

        // install dependencies
        stage('Install Dependencies'){
            steps{
                script{
                    sh """
                        npm install
                    """
                }
            }
        }
        
        // perform unit testing
        stage('Unit Testing') {
            steps {
                script {
                   sh """
                        echo "unit tests"
                   """
                }
            }
        }

        // build docker image and push to ECR
        stage('Docker Build'){
            steps{
                script{
                    withAWS(credentials: 'aws-auth', region: 'us-east-1') {
                       sh """
                            aws ecr get-login-password --region ${REGION} | docker login --username AWS --password-stdin ${ACC_ID}.dkr.ecr.us-east-1.amazonaws.com
                            docker build -t ${ACC_ID}.dkr.ecr.us-east-1.amazonaws.com/${PROJECT}/${COMPONENT}:${appVersion} .
                            docker push ${ACC_ID}.dkr.ecr.us-east-1.amazonaws.com/${PROJECT}/${COMPONENT}:${appVersion}
                        """
                    }
                }
            }
        }

        stage('Test') {
            steps {
                echo ' considering UNIT Testing..'
            }
        }

        // Devlopers does't trigger code always, they trigger the code when they are confident
        stage('Trigger Deploy') {
            when{
                expression { params.DEPLOY }
            }
            steps {
                script{
                   build job: 'catalogue-cd',
                    parameters: [
                        string(name: 'appVersion', value: "${appVersion}"),
                        string(name: 'deploy_to', value: 'dev')
                    ],
                   propagate: false,
                   wait: false
                }                
            }
        }
    }

    post { 
        always { 
            echo 'I will always say Hello again!'
            deleteDir()
        }
        success { 
            echo 'Hello Success'
        }
        failure { 
            echo 'Hello Failure'
        }
    }
}

