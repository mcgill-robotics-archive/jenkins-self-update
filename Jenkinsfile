pipeline {
  agent any
  environment {
    JENKINS_URL = "http://127.0.0.1:8080"
    USERNAME = "mcgillrobotics"
    PAT_FILE = "/home/jenkins/pat_file"
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
          java -jar jenkins-cli.jar -s ${JENKINS_URL} list-plugins |
            grep ')$' |
            cut -d' ' -f1 |
            tr '\n' ' '
        )"

        if [ ! -z "${UPDATE_LIST// }" ]; then
          java -jar jenkins-cli.jar -s ${JENKINS_URL} install-plugin ${UPDATE_LIST} --username ${USERNAME} --password-file ${PAT_FILE}
          java -jar jenkins-cli.jar -s ${JENKINS_URL} safe-restart --username ${USERNAME} --password-file ${PAT_FILE}
        else
          echo "No updates available"
        fi
        '''
      }
    }
  }
}
