pipeline {
  agent {
    node {
      label 'python'
    }
    
  }
  stages {
    stage('pre_ci') {
      steps {
        sh 'docker build -t ansible/awx_devel -f tools/docker-compose/Dockerfile .'
        sh 'docker tag ansible/awx_devel gcr.io/ansible-tower-engineering/awx_devel:latest'
      }
    }
    stage('ci') {
      steps {
        sh 'cp -R . /awx_devel'
        sh 'pip install -U docker-compose'
        sh 'docker-compose -f tools/docker-compose/unit-tests/docker-compose-shippable.yml build --build-arg TAG=latest unit-tests'
        sh '''docker-compose -f tools/docker-compose/unit-tests/docker-compose-shippable.yml run unit-tests "make ${AWX_BUILD_TARGET}" 
'''
        sh 'python tools/docker-compose/unit-tests/collect_shippable_results.py'
      }
    }
  }
}