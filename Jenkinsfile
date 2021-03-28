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
      parallel {
        stage('Code Analysis') {
          steps {
            withSonarQubeEnv('sonar') {
              powershell 'gradle sonarqube'
            }

            waitForQualityGate true
          }
        }

        stage('Test Reporting') {
          steps {
            cucumber(fileIncludePattern: 'https://github.com/LamiaMed/TP_OGL/tree/master/reports/', jsonReportDirectory: 'reports/', buildStatus: 'https://github.com/LamiaMed/TP_OGL/tree/master/reports/', reportTitle: 'Reportjenkins')
          }
        }

      }
    }

    stage('Deployment') {
      steps {
        powershell 'gradle publish'
      }
    }

    stage('Slack Notification') {
      steps {
        slackSend(baseUrl: 'https://hooks.slack.com/services/', token: 'T01M5ETUS22/B01SZLNHYG1/8V0tkFvgfVRFuKfSvQKaqCNQ', message: 'Hi slack channel', username: 'lam', channel: '#general', attachments: 'Hello', blocks: 'hiiii', sendAsText: true, teamDomain: 'tpoglgroupe.slack.com')
      }
    }

  }
}