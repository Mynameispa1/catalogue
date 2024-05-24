pipeline {
    agent {
    node {
        label 'AGENT-1'
        }
    }

    environment { 
        packageVersion = ''
        nexusURL = '172.31.36.12:8081'
    }

    options {
        timeout(time: 1, unit: 'HOURS') 
        disableConcurrentBuilds() 
    }

    // build
    stages {
          stage('Get the version') {
            steps {
                script {
                    def packageJson = readJSON file: 'package.json'
                    packageVersion = packageJson.version
                    echo "application version: $packageVersion"
                }
            }
        }

        stage('Install dependencies') {
            steps {
                sh """
                    npm install
                """
            }
        }

          stage('Build') {
            steps {
                sh """
                    ls -la
                    zip -q -r catalogue.zip ./* -x ".git" -x "*.zip"
                    ls -ltr
                """
            }
        }

        stage('Publish Artifact') {
            steps {
                 nexusArtifactUploader(
                    nexusVersion: 'nexus3',
                    protocol: 'http',
                    nexusUrl: "${nexusURL}",
                    groupId: 'com.roboshop',
                    version: "${packageVersion}",
                    repository: 'catalogue',
                    credentialsId: 'nexus-auth',
                    artifacts: [
                        [artifactId: 'catalogue',
                        classifier: '',
                        file: 'catalogue.zip',
                        type: 'zip']
                    ]
                )
            }
        }
        
        stage('Deploy') {
            steps {
                sh """
                    echo 'Here I am writing shell script'
                    echo '$Name'
                """
                // echo 'Deploying....'
            }
        }
    }
    // post
     post { 
        always { 
            echo 'I will always say Hello again!'
        }

         failure { 
            echo 'This run when pipeline failed!'
        }

         success { 
            echo 'This will run when pipeline is successful'
        }
    }
}