pipeline {
    agent {
        node{
            label 'AGENT-1'
        }
    }
    environment {
         COURCE ="jenkins"
         appVersion = ""

    }
     
    stages {
        stage('Build') {
            steps {
                script{

                
                sh """
                echo "building"
                 """
                }
            }
        }
        
      stage('Read Version') {

            steps {
                script {
             def packageJSON = readJSON file: 'package.json'  // your package.json content will be stored here defining and storing the package.json file
                appVersion = packageJSON.version // accesesing the version of the catalogue(app)
                echo "app version: ${appVersion}" //printing the version
                }
            }
        }

        stage('Test') {
            steps {
                script{
                sh """
                
                echo "Testing"
                 """
                }
            }
        }
        stage('Deploy') {
          
           
            
            steps {
                script{
                sh """
                echo "Deploye"
                 """
                }
            }
    
        }
    }
    post {
     success {
            echo "hello i will run when pipeline success"
        }
    }
    
    }
