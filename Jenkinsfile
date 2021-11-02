pipeline {
    agent any
    tools {
        maven 'mvn383'
    }
    stages {
        stage('Unit test') {
            steps {
                echo 'Hello, Maven'
                sh 'mvn --version'
                sh 'mvn test'
            }
        }
        stage('Package') {
            steps {
                echo 'Hello, JDK'
                sh 'java -version'
                sh  'mvn package -DskipTest'
            }
        }


        stage('Build and run docker image') {
            steps {
                  sh 'docker build -t zizodk/test:v1.0.${BUILD_NUMBER} .'

                  sh 'docker run --name test${BUILD_NUMBER} -d -p 8088:8088 zizodk/test:v1.0.${BUILD_NUMBER}'
            }
        }

        stage('Push docker image') {
            steps {
                  withCredentials([usernamePassword (credentialsId:"dockerhub",passwordVariable :'DOCKER_PASSWORD',usernameVariable :'DOCKER_USERNAME')]){
                    sh 'docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD '
                    sh 'docker push freemanpolys/test:v1.0.${BUILD_NUMBER}'
                  }
            }
        }  


    }
    


    post {
         always {
             junit 'target/surefire-reports/*.xml'
             archiveArtifacts artifacts: 'target/*.jar'
         }
    }
}
