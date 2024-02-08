pipeline {
    agent {
    node {
        label 'Agent-1'
        }
    }

      environment { 
        packageVersion = ''
        //nexusURL = '172.31.5.95:8081'
    }

     options {
        timeout(time: 1, unit: 'HOURS')
        disableConcurrentBuilds()       //to run only one build at a time
    }

    // parameters {
    //     string(name: 'PERSON', defaultValue: 'Mr Jenkins', description: 'Who should I say hello to?')

    //     text(name: 'BIOGRAPHY', defaultValue: '', description: 'Enter some information about the person')

    //     booleanParam(name: 'TOGGLE', defaultValue: true, description: 'Toggle this value')

    //     choice(name: 'CHOICE', choices: ['One', 'Two', 'Three'], description: 'Pick something')

    //     password(name: 'PASSWORD', defaultValue: 'SECRET', description: 'Enter a password')
    //}
  // Build stage
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
                ls -la
                """
            }
        }

        stage('Test') { 
            steps {
                echo 'Testing..'
            }
        }
        stage('Deploy') { 
            steps {
                sh """
                echo "Here I wrote shell script"
                // echo "$GREETING"
                // env
                """ 
            }
        }

    //     stage('check params'){
    //         steps {
    //             sh """
    //                 echo "Hello ${params.PERSON}"

    //                 echo "Biography: ${params.BIOGRAPHY}"

    //                 echo "Toggle: ${params.TOGGLE}"

    //                 echo "Choice: ${params.CHOICE}"

    //                 echo "Password: ${params.PASSWORD}"
    //             """
    //         }
    //     }
    // }
    //post build
    post { 
        always { 
            echo 'I will always say Hello again!'
            deleteDir()
        }

        failure { 
            echo 'This is Failed...!'
        }

        success {
            echo 'This is success...!'
        }
    }
}    


   