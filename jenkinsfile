pipeline {
    agent {
        node {
          label 'jenkins-maven-slave'
        }
    }

environment{
    PATH = "/opt/apache-maven-3.9.6/bin:$PATH"
}

    stages {
        stage('Build') {
            steps {
               sh 'mvn clean deploy'
            }
        }
    }
}


