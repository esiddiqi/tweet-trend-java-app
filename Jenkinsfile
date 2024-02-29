def registry = 'https://emaad.jfrog.io'
def imageName = "emaad.jfrog.io/trend-java-pipeline-docker/api-loan-app"
def version   = '2.1.4'

pipeline {
    agent {
        node {
            label 'master'
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

        stage('Build') {
            steps {
                echo "------Building the Code-------"
                sh 'mvn clean deploy'
                echo "------Build complete-------"
            }
        }





        stage('Test') {
            steps {
                echo "------Unit Test started-------"
                sh 'mvn surefire-report:report'
                echo "------Unit Test Completed-------"
            }
        }


        stage("Jar Publish") {
        steps {
            script {
                    echo '<--------------- Jar Publish Started --------------->'
                     def server = Artifactory.newServer url:registry+"/artifactory" ,  credentialsId:"artifactory-cred"
                     def properties = "buildid=${env.BUILD_ID}";
                     def uploadSpec = """{
                          "files": [
                            {
                              "pattern": "target/*.jar",
                              "target": "trend-java-pipeline-javarepo/",
                              "flat": "false",
                              "props" : "${properties}",
                              "exclusions": [ "*.original", "*.exec"]
                            }
                         ]
                     }"""
                     def buildInfo = server.upload(uploadSpec)
                     buildInfo.env.collect()
                     server.publishBuildInfo(buildInfo)
                     echo '<--------------- Jar Publish Ended --------------->'

            }
        }
    }




    stage("Docker build"){
      steps{
        script{
          echo '<----------- Docker Build Started----------->'
          app = docker.build(imageName+":"+version)
          echo '<----------- Docker Build Ends-------------->'

        }
      }
    }


    stage("Docker Publish"){
      steps{
        script{
          echo '<-----------------Docker Publish Started---------------------->'
          docker.withRegistry(registry, "artifactory-cred"){
            app.push()
          }

        }
      }
    }

    stage("Deploy Helm Chart"){
     steps{
       script{
        echo 'STARTED HELM DEPLOY'
        sh 'helm install java-app /home/ec2-user/java-app-0.1.0.tgz'
        echo "HELM DEPLOY COMPLETED"
       }

     }
    }







    }
}