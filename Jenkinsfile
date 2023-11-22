pipeline {
    agent any

      stages {
          stage('build') {
              steps {
                  echo 'building the software'
                  sh 'npm install'
              }
          }
          stage('test') {
              steps {
                  echo 'testing the software'
                  sh 'npm test'
              }
          }

          stage('deploy') {
              steps {
                withCredentials([sshUserPrivateKey(credentialsId: "jenkins-dserver-pem", keyFileVariable: 'sshkey')]){
                  echo 'deploying the software'
                  sh '''#!/bin/bash
                  echo "Creating .ssh"
                  mkdir -p /var/lib/jenkins/.ssh
                  ssh-keyscan 18.116.59.129 >> /var/lib/jenkins/.ssh/known_hosts
                  rsync -avz --exclude  '.git' --delete -e "ssh -i $sshkey" ./ ubuntu@18.116.59.129:/app/
                  ssh -i $sshkey ubuntu@18.116.59.129 "sudo systemctl restart nodeapp"
                  '''
              }
          }
      }
    }
}
