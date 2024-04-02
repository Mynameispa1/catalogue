pipeline {
    agent {
    node {
        label 'AGENT-1'
    }
}
    environment { 
        packageVersion = ''
        nexusURL = '172.31.84.200:8081' // change the url as per your details
    }
    options {
        timeout(time: 1, unit: 'HOURS') 
        disableConcurrentBuilds()
        ansiColor('xterm')
    }

    parameters {
        string(name: 'PERSON', defaultValue: 'Mr Jenkins', description: 'Who should I say hello to?')

        text(name: 'BIOGRAPHY', defaultValue: '', description: 'Enter some information about the person')

        booleanParam(name: 'TOGGLE', defaultValue: true, description: 'Toggle this value')

        choice(name: 'CHOICE', choices: ['One', 'Two', 'Three'], description: 'Pick something')

        password(name: 'PASSWORD', defaultValue: 'SECRET', description: 'Enter a password')
    }
   //   build
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
                  ls -lrt
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
                sh '''
                    echo "Here I am writing shell script"
                    echo "$Greeting"
                '''
            }
        }
        stage('check params'){
            steps {
                sh """
                    echo "Hello ${params.PERSON}"

                    echo "Biography: ${params.BIOGRAPHY}"

                    echo "Toggle: ${params.TOGGLE}"

                    echo "Choice: ${params.CHOICE}"

                    echo "Password: ${params.PASSWORD}"
                """
            }
        }
    }

  //post build
  post { 
        always { 
            echo 'I will always say Hello again!'
            deleteDir()
        }

    failure {  
            echo 'This run is the pipepline get fail...'
        }
    success {
        echo 'This run if the pipeline success...'
    }    
  }

}