pipeline {
  agent {
    kubernetes {
      label 'nodejs'
      yamlFile 'nodejs-pod.yml'
    }
  }
  options {
    buildDiscarder(logRotator(numToKeepStr: '10'))
    skipDefaultCheckout true
  }
  stages {
    stage('Setup') {
      steps {
        container('nodejs') {
         checkout scm
         sh 'yarn install'
        }
      }
    }
    stage('Tests') {
      parallel {
        stage('Lint') {
          steps {
            container('nodejs') {
              sh 'pwd;ls'
              sh 'yarn lint'
            }
          }
        }
        stage('Unit tests') {
          steps {
            container('nodejs') {
              sh 'ls'
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
        echo "TODO - build and push image"
      }
    }
  }
}
