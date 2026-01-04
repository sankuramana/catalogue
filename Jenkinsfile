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
                sh """
                }
            }
        }
        
      stage('Read Version') {
            steps {
             def packageJSON = readJSON file: 'package.json'  // your package.json content will be stored here defining and storing the package.json file
                appVersion = packageJSON.Version // accesesing the version of the catalogue(app)
                echo "app version: ${appVersion}" //printing the version
            }
        }

        stage('Test') {
            steps {
                script{
                sh """
                
                echo "Testing"
                sh """
                }
            }
        }
        stage('Deploy') {
          
           
            
            steps {
                script{
                sh """
                echo "Deploye"
                sh """
                }
            }
    
        }
    }
     success {
            echo "hello i will run when pipeline success"
        }
    
    }
