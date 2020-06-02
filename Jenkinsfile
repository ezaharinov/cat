pipeline {
  agent any
  stages {
    stage('Server') {
      agent {
        docker {
          image 'maven:3.6-jdk-8'
        }

      }
      steps {
        sh '''cd api/
echo "Building Server code..."
mvn -version
mvn clean install -Dmaven.test.skip=true
pwd'''
        stash(name: 'server', allowEmpty: true, includes: '**/target/*')
        echo 'Done!! '
      }
    }

    stage('Client') {
      agent {
        docker {
          image 'node:latest'
          args '-u 0:0'
        }

      }
      steps {
        sh '''cd web/
npm install'''
        echo 'Done!'
        stash(name: 'node', includes: '**/*')
      }
    }

    stage('testserver') 
    dir('/usr/local/opt')
    {
      steps {
        sh '''pwd
        unstash 'server'
        unstash 'node'
      }
    }

  }
}