pipeline {
    agent { label 'linux' }

    stages {
        stage ('Checkout') {
          steps {
            git 'https://github.com/louiekwan/spring-petclinic.git'
          }
        }
        stage('Build') {
            //agent { docker 'maven:3.5-alpine' }
      agent {
        docker {
          image 'maven:3-alpine'
        }
      }
            steps {
                sh 'mvn clean package'
                junit '**/target/surefire-reports/TEST-*.xml'
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            }
        }
        stage('Deploy') {
          steps {
            input 'Do you approve the deployment?'
            sh 'scp target/*.jar jenkins@192.168.50.10:/opt/pet/'
            sh "ssh jenkins@192.168.50.10 'nohup java -jar /opt/pet/spring-petclinic-1.5.1.jar &'"
          }
        }
    }
}
