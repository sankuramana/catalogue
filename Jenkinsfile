pipeline {
    agent {
        node{
            label 'AGENT-1'
        }
    }
    environment {
         COURCE ="jenkins"
         appVersion = ""
          ACC_ID = "375553085606"
         PROJECT = "roboshop"
         COMPONENT = "catalogue"

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
        stage( 'install dependencies '){

            steps{
                script{
                    sh """
                      npm install
                    """

                }
            }
        }


         stage('Build Image') {
            steps {
                script{
                    withAWS(region:'us-east-1',credentials:'aws-creds') {
                        sh """

                        """
                    }
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
