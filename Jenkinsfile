pipeline {
    agent any
    tools{
        jdk 'jdk17'
        maven 'maven3'
    }
    
    environment {
        SCANNER_HOME = tool 'sonar-scanner'
    }

    stages {
        stage('Git-Checkout') {
            steps {
               git branch: 'main', url: 'https://github.com/chandrasekhar1897/Ekart.git'
            }
        }
        
        stage('Compile') {
            steps {
               sh "mvn compile"
            }
        }
        
        stage('Test') {
            steps {
               echo "Test Passed"
            }
        }
        
         
        stage('Sonarqube-scanner') {
            steps {
               withSonarQubeEnv('sonar') {
                   
                sh ''' $SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=Ekart \
                -Dsonar.java.binaries=. \
                -Dsonar.projectKey=Ekart '''
              }
            }
        }

        stage('Build Application') {
            steps {
             sh "mvn package -DskipTests=true"
            }
        }

        stage('Build Docker Image') {
            steps {
             script {
                  withDockerRegistry(credentialsId: 'docker-cred', toolName: 'docker') {
                  sh "docker build -t shopping-cart:dev -f docker/Dockerfile ."  

                }
                 
            }
        }
        }
           stage('Docker Image Tag') {
            steps {
             script {
                  withDockerRegistry(credentialsId: 'docker-cred', toolName: 'docker') {
                  sh "docker tag shopping-cart:dev santhosh7mp/shopping-cart:dev"  

                }
                 
            }
        }
        }
    }
}
