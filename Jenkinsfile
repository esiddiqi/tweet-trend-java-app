pipeline {
    agent {
        node {
          label 'jenkins-maven-slave'
        }
    }

    environment {
       PATH = "/opt/apache-maven-3.9.6/bin:$PATH"
    }

    stages {
        stage('Pull Code') {
            steps {

              echo "-------------Pulling the code-----------------"
              git branch: 'main', url: 'https://github.com/esiddiqi/tweet-trend-java-app.git'

            }
        }
    }
}


