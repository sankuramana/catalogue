pipeline {
    agent {
        node{
            label 'AGENT-1'
        }
    }
    environment {
         COURCE ="jenkins"
         appVersion = ""
          ACC_ID = "060002399633"
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


        stage('Test') {
            steps {
                script{
                sh """
                
                echo "Testing"
                 """
                }
            }
        }
        stage('Build & Push Docker Image') {
    steps {
        script {
            withAWS(region:'us-east-1',credentials:'aws-creds') {
                sh """
                    # Ensure repo exists
                    aws ecr describe-repositories --repository-names ${PROJECT}/${COMPONENT} || \
                    aws ecr create-repository --repository-name ${PROJECT}/${COMPONENT}

                    # Login to ECR
                    aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin ${ACC_ID}.dkr.ecr.us-east-1.amazonaws.com

                    # Build Docker image
                    sudo docker build -t ${ACC_ID}.dkr.ecr.us-east-1.amazonaws.com/${PROJECT}/${COMPONENT}:${appVersion} .

                    # Verify image
                    sudo docker images

                    # Push to ECR
                    sudo docker push ${ACC_ID}.dkr.ecr.us-east-1.amazonaws.com/${PROJECT}/${COMPONENT}:${appVersion}
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
