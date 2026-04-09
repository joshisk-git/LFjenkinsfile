#pipeline {
 #   agent any

#   environment {
 #       APP_VERSION = '1.0.0'
  #  }
#
 #   stages {
  #      stage('Checkout') {
   #         steps {
    #            echo 'Cloning repository...'
     #           checkout scm
      #      }
       # }

        #stage('Build') {
         #   steps {
          #      echo "Building version ${APP_VERSION}"
#                sh 'chmod +x app.sh'
           #     sh './app.sh'
            #}
        #}

       # stage('Test') {
        #    steps {
         #       echo 'Running tests...'
          #      sh 'chmod +x test.sh'
           #     sh './test.sh'
           # }
       # }

       # stage('Archive') {
        #    steps {
         #       echo 'Archiving artifacts...'
          #      archiveArtifacts artifacts: 'app.sh', fingerprint: true
           # }
       # }
   # }

   # post {
    #    success { echo "Build #${BUILD_NUMBER} succeeded!" }
     #   failure { echo "Build #${BUILD_NUMBER} failed. Check console." }
      #  always  { cleanWs() }
   # }
#}

#groovy



node {

    def APP_VERSION = "1.0.0"

    try {

        stage('Checkout') {
            echo 'Cloning repository...'
            checkout scm
        }

        stage('Build') {
            echo "Building version ${APP_VERSION}"
            sh 'chmod +x app.sh'
            sh './app.sh'
        }

        stage('Test') {
            echo 'Running tests...'
            sh 'chmod +x test.sh'
            sh './test.sh'
        }

        stage('Archive') {
            echo 'Archiving artifacts...'
            archiveArtifacts artifacts: 'app.sh', fingerprint: true
        }

        echo "Build #${env.BUILD_NUMBER} succeeded!"

    } catch (Exception e) {

        echo "Build #${env.BUILD_NUMBER} failed. Check console."
        throw e

    } finally {

        cleanWs()
    }
}
