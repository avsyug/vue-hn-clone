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
         checkout scm
         sh 'yarn install'
         stash name: 'node', includes: '**', excludes: '**/.git,**/.git/**'
        }
      }
    }
    stage('Tests') {
      parallel {
        stage('Lint') {
          steps {
            container('nodejs') {
              unstash name: 'node'
              sh 'pwd;ls'
              sh 'yarn lint'
            }
          }
        }
        stage('Unit tests') {
          steps {
            container('nodejs') {
              unstash name: 'node'
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
        unstash name: 'node'
        echo "TODO - build and push image"
      }
    }
  }
}
