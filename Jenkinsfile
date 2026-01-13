pipeline {
    // These are pre build section
    agent  {
        node {
            label 'AGENT-1'
        }
    }
    
    environment {
        COURSE = "Jenkins CICD"
        appVersion = ""
        ACC_ID = "850017502474"
        PROJECT = "roboshop"
        COMPONENT = "catalogue"
    }
    options {
        timeout(time: 10, unit: 'MINUTES') 
        disableConcurrentBuilds()
    }

    // This is Build Section
    stages {
        stage('Read Version') {
            steps {
                script { 
                        def packageJSON = readJSON file: 'package.json'
                        appVersion = packageJSON.version
                        echo "app version: ${appVersion}"
                }
            }
        }
        stage('Install Dependencies') {
            steps {
                script {
                    sh """
                        npm install
                    """
                }
            }
        }
        stage('Image Build') {
            steps {
                script {
                    withAWS(region:'us-east-1',credentials:'aws-creds') {
                    sh """
                        aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin ${ACC-ID}.dkr.ecr.us-east-1.amazonaws.com
                        docker build ${ACC_ID}.dkr.ecr.us-east-1.amazonaws.com/${PROJECT}/${COMPONENT}:${appVersion}
                        docker images
                        docker push ${ACC_ID}.dkr.ecr.us-east-1.amazonaws.com/${PROJECT}/${COMPONENT}:${appVersion}

                    """
                }
            }
        }
    }
}
    // This are post build section
    post {
        always {
            echo 'I will always say Hello again!'
            cleanWs()
        }
        success {
            echo "I will Run if Success"
        }
        failure {
            echo "I will Run if Failure"
        }
        aborted {
            echo " Jenkins Pipelines are aborted "
        }
    }
}