def COLOR_MAP = ['SUCCESS': 'good', 'FAILURE': 'danger', 'UNSTABLE': 'danger', 'ABORTED': 'danger']

pipeline {
  agent {
    docker {
        image "ruby:alpine"
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
            color: COLOR_MAP[currentBuild.currentResult],
            message: "*${currentBuild.currentResult}:* Job ${env.JOB_NAME} build ${env.BUILD_NUMBER}\n Mais informações acesse: ${env.BUILD_URL}"
        }
      }
    }
  }
}
