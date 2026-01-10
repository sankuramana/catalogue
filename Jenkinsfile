pipeline {
    agent {
        node {
            label 'AGENT-1'
        }
    }

    environment {
        COURSE     = "jenkins"
        ACC_ID     = "060002399633"
        PROJECT    = "roboshop"
        COMPONENT  = "catalogue"
        AWS_REGION = "us-east-1"
    }

    stages {

        stage('Build') {
            steps {
                sh 'echo "building"'
            }
        }

        stage('Read Version') {
            steps {
                script {
                    def packageJSON = readJSON file: 'package.json'
                    env.APP_VERSION = packageJSON.version   // ✅ IMPORTANT
                    echo "App version: ${env.APP_VERSION}"
                }
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Unit testing') {
            steps {
                sh """
                npm test
                """
            }
        }
      //Here you need to select scanner tool and send the analysis to server
         stage('Sonar Scan'){
            environment {
                def scannerHome = tool 'sonar-8.0'
            }
            steps {
                script{
                    withSonarQubeEnv('sonar-server') {
                        sh  "${scannerHome}/bin/sonar-scanner"
                    }
                }
            }
         }
                 stage('Quality Gate') {
            steps {
                timeout(time: 1, unit: 'HOURS') {
                    // Wait for the quality gate status
                    // abortPipeline: true will fail the Jenkins job if the quality gate is 'FAILED'
                    waitForQualityGate abortPipeline: true 
                }
            }
                 }

        stage('Build & Push Docker Image') {
            steps {
                withAWS(region: "${AWS_REGION}", credentials: 'aws-creds') {
                    sh """
                        set -e

                        aws sts get-caller-identity

                        aws ecr describe-repositories \
                          --repository-names ${PROJECT}/${COMPONENT} \
                          --region ${AWS_REGION} || \
                        aws ecr create-repository \
                          --repository-name ${PROJECT}/${COMPONENT} \
                          --region ${AWS_REGION}

                        aws ecr get-login-password --region ${AWS_REGION} | \
                        docker login --username AWS --password-stdin \
                        ${ACC_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com

                        docker build -t \
                        ${ACC_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${PROJECT}/${COMPONENT}:${APP_VERSION} .

                        docker images

                        docker push \
                        ${ACC_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${PROJECT}/${COMPONENT}:${APP_VERSION}
                    """
                }
            }
        }
    }

    post {
        success {
            echo "Pipeline completed successfully 🎉"
        }
        failure {
            echo "Pipeline failed ❌"
        }
    }
}
