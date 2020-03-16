pipeline {
  agent any
  environment {
    JENKINS_URL = "http://127.0.0.1:8080"
    JENKINS_USER_ID = "mcgillrobotics"
    JENKINS_API_TOKEN = credentials("jenkins-api-token")
  }
  triggers {
    cron('0 8 * * *')
  }
  stages {
    stage('Download CLI') {
      steps {
        sh "wget ${JENKINS_URL}/jnlpJars/jenkins-cli.jar"
      }
    }
    stage('Update Plugins') {
      steps {
        sh '''#!/bin/bash
        set -e

        UPDATE_LIST="$(
          java -jar jenkins-cli.jar list-plugins |
            grep ')$' |
            cut -d' ' -f1 |
            tr '\n' ' '
        )"

        if [ ! -z "${UPDATE_LIST// }" ]; then
          java -jar jenkins-cli.jar install-plugin ${UPDATE_LIST}
          java -jar jenkins-cli.jar safe-restart
        else
          echo "No updates available"
        fi
        '''
      }
    }
  }
}
