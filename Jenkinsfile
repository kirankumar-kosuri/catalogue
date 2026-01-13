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
                    echo "app version : ${appVersion}"
                }
            }
        }
        stage('Install Dependence') {
            steps {
                script {
                    sh """
                        npm install
                    """
                }
            }
        }
        stage('Build Image') {
           
            steps {
                script {
                    sh """
                        docker build -t catalogue:${appVersion} .
                        docker images
                    """
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