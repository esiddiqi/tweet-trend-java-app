pipeline {
    agent {
        node {
          label 'jenkins-maven-slave'
        }
    }

    environment {
       PATH = "/opt/apache-maven-3.9.6/bin:$PATH"
       GIT_HOME = "/usr/bin/git"
    }


    stages {
        stage('Build') {
            steps {
               sh 'mvn clean deploy'
            }
        }
    }
}


