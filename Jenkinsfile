pipeline {
  agent {
    docker {
        image "ruby:alpine"
        args "--network=skynet"
    }
  }
  stages {
    stage("Build") {
      steps {
        sh "chmod +x build/alpine.sh"
        sh "./build/alpine.sh"
        sh "bundle install"
      }
    }
    stage("Test") {
      steps {
        sh "bundle exec cucumber -p ci"
      }
      post {
        always {
          cucumber fileIncludePattern: '**/*.json', jsonReportDirectory: 'log', sortingMethod: 'ALPHABETICAL'
          slackSend channel: "#jenkins",
            color: 'good',
            message: "*${currentBuild.currentResult}:* Job ${env.JOB_NAME} build ${env.BUILD_NUMBER}\n Mais informações acesse: ${env.BUILD_URL}"
        }
      }
    }
  }
}
