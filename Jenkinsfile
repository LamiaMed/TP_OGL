pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        powershell 'gradle build'
        powershell 'gradle javadoc'
        archiveArtifacts 'build/libs/*.jar'
        archiveArtifacts 'build/docs/javadoc/*'
      }
    }

    stage('Mail Notification') {
      steps {
        mail(subject: 'Notification', body: 'Hey Aya you are notified', from: 'hl_medjahed@esi.dz', to: 'ga_bendjeddou@esi.dz')
      }
    }

    stage('Code Analysis') {
      steps {
        waitForQualityGate true
      }
    }

  }
}