pipeline {
    agent any
    tools {
        maven 'maven368'
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
    }
    
    post {
         always {
             junit 'target/surfire-reports/**/*.xml'
             archiveArtifacts artifacts: 'target/*.jar'
         }
    }
}
