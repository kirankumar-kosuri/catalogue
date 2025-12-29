pipeline {
    // Pre-Build-Section
    agent {
        node {
        label 'AGENT-1'
        }
    }
    environment{
        COURSE = "Jenkins"
        appVersion = ""
    }
    options {
        //timeout(time: 10, unit: 'SECONDS') 
        disableConcurrentBuilds()
    }
    // Build-Section
    stages {
        stage('Read Version') {
            steps {
                script { 
                    def packageJson = readJSON file: 'package.json'
                    appVersion = packageJSON.version
                    echo "app version: ${appVersion}"
                    }
                
            }
        }
        stage('Test') {
            steps {
               script {
                    sh """
                        echo "Testing"
                        echo $COURSE
                    """
               }
            }
        }
        stage('Deploy') {
            // input {
            //     message "Should we continue?"
            //     ok "Yes, we should."
            //     submitter "alice,bob"
            //     parameters {
            //         string(name: 'PERSON', defaultValue: 'Mr Jenkins', description: 'Who should I say hello to?')
            //     }
             // }
            when {
                expression {"$params.DEPLOY"}
            }
            
            steps {
                script {
                    sh """
                        echo "Deploying"
                        echo $COURSE
                    """
                }
            }
        }
    }


    // Post-Build-Section
    post{
        always{
            echo 'I will always say hello Again!'
            cleanWs()
        }
        success{
            echo 'I will run if success'
        }
        failure{
            echo 'I will run if failure'
        }
        // aborted{
        //     echo 'pipelines are aborted'
        // }
    }
}


