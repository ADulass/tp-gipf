pipeline {
  agent any
  environment {
    SONAR_TOKEN = credentials('sonar-token')
  }
  stages {
    stage('Checkout') {
      steps { checkout scm }
    }
    stage('Install') {
      steps { sh 'npm ci' }
    }
    stage('Build') {
      steps { sh 'npm run build' }
    }
    stage('SonarQube Analysis') {
      steps {
        withSonarQubeEnv('SonarQube') {
          sh 'npm run sonar'
        }
      }
    }
    stage('Quality Gate') {
      steps {
        timeout(time: 1, unit: 'HOURS') {
          waitForQualityGate abortPipeline: true
        }
      }
    }
  }
  post {
    failure {
      echo 'Qualité non atteinte – pipeline aborted.'
    }
  }
}
