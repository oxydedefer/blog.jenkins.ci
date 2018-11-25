
pipeline {
    agent {dockerfile true}
     environment {
        PACKAGE_NAME = "packageseb"
        TAG = sh 'git describe --abbrev=0 --tags'
    }
    stages{
        stage("Build"){
            steps{
                echo "build always"
            }
        }
        stage("Test"){
            steps{
                echo "make tests always"
            }
        }
        stage("Deploy on Alpha registry"){
         when { tag "*-alpha" }
            steps {
                echo 'Deploying only because this commit is tagged in ALPHA'

            }

        }
        stage("Deploy on Beta registry"){
         when { tag "*-beta" }
            steps {
                 docker.withRegistry('http://localhost:5000')
                 {
                    def customImage = docker.build("env.PACKAGE_NAME:${env.TAG}")
                    customImage.push()

                    customImage.push('latest')

                 }
            }
        }
        stage("Deploy on production regitry"){
         when { tag "*-release" }
            steps {
                echo 'Deploying only because this commit is tagged in release'

            }

        }



    }
    post {
      unstable {
         slackSend color: "warning", message: "Job: ${env.JOB_NAME} with buildnumber ${env.BUILD_NUMBER} was unstable"
      }
      success {
        slackSend color: "good", message: "Job: ${env.JOB_NAME} with buildnumber ${env.BUILD_NUMBER} was successful"
      }
      failure {
        slackSend color: "danger", message: "Job: ${env.JOB_NAME} with buildnumber ${env.BUILD_NUMBER} was failed"
      }
    }
}