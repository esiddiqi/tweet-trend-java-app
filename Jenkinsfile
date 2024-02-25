pipeline {
    agent {
        node {
          label 'jenkins-maven-slave'
        }
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


