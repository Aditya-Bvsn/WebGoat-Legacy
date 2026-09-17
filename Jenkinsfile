pipeline {
  agent any

  tools {
    maven 'mvn3916'
    jdk 'jdk8'
  }

  environment {
    ARTEFACT_NAME = "${WORKSPACE}/target/WebGoat-${BUILD_VERSION}.war"
    IQ_SCAN_URL = ''
  }

  stages {
    stage('Verify Java') {
      steps {
        sh 'java -version'
        sh 'mvn -version'
      }
    }

    stage('Build') {
      steps {
        sh 'mvn -B -Dproject.version=$BUILD_VERSION -Dmaven.test.failure.ignore clean package'
      }
      post {
        success {
          echo 'Now archiving...'
          archiveArtifacts artifacts: '**/target/*.war'
        }
      }
    }

    stage('Nexus IQ Scan') {
      steps {
        script {
          def policyEvaluation = nexusPolicyEvaluation(
            advancedProperties: '',
            enableDebugLogging: false,
            failBuildOnNetworkError: false,
            failBuildOnScanningErrors
