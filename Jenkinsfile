pipeline {
  agent any
  environment {
    SONAR_TOKEN = credentials('sqp_6fa82d19f9cbbaba7bd5f29d0b504d63596348f')
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
