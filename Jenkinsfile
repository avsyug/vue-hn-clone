pipeline {
  agent {
    kubernetes {
      label 'nodejs'
      yamlFile 'nodejs-pod.yml'
    }
  }
  options {
    buildDiscarder(logRotator(numToKeepStr: '2'))
    skipDefaultCheckout true
  }
  stages {
    stage('Setup') {
      steps {
        container('nodejs') {
         sh 'yarn install'
        }
      }
    }
    stage('Tests') {
      parallel {
        stage('Lint') {
          steps {
            container('nodejs') {
              sh 'yarn lint'
            }
          }
        }
        stage('Unit tests') {
          steps {
            container('nodejs') {
              sh 'yarn test'
            }
          }
        }
      }
    }
    stage('Build and Push Image') {
      when {
        beforeAgent true
        branch 'master'
      }
      steps {
        checkout scm
        echo "TODO - build and push image"
      }
    }
  }
}
