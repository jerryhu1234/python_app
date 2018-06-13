pipeline {
  agent none
  stages {
    stage('Build') {
      agent {
        docker {
          image 'python:2-alpine'
          args '-v /var/run/docker.sock'
        }

      }
      steps {
        sh 'python -m py_compile sources/add2vals.py sources/calc.py'
      }
    }
    stage('Test') {
      agent {
        docker {
          image 'qnib/pytest'
          args '-v /var/run/docker.sock'
        }

      }
      steps {
        sh 'py.test --verbose --junit-xml test-reports/results.xml sources/test_calc.py'
      }
      post {
        always {
          junit 'test-reports/results.xml'

        }

      }
    }
    stage('Deliver') {
      agent {
        docker {
          image 'cdrx/pyinstaller-linux:python2'
          args '-v /var/run/docker.sock'
        }

      }
      steps {
        sh 'pyinstaller --onefile sources/add2vals.py'
        archiveArtifacts 'dist/add2vals'
      }
    }
  }
}