pipeline {
  agent none
  stages {
    stage('Requirements') {
      agent {
        docker {
          image 'python:3.6-stretch'
          args '-p 3001:3000'
        }

      }
      environment {
        HOME = '.'
      }
      steps {
        sh 'virtualenv venv && . venv/bin/activate && pip3 install -r requirements.txt'
        sh 'pip3 install flask'
        sh 'pip3 install bokeh'
      }
    }

    stage('Deploy') {
      agent {
        label 'master'
      }
      options {
        skipDefaultCheckout()
      }
      steps {
        sh 'rm -rf /var/www/monitoreo'
        sh 'mkdir /var/www/monitoreo'
        sh 'cp -Rp . /var/www/monitoreo'
        sh 'docker stop monitoreo || true && docker rm monitoreo || true'
        sh 'docker run -dit --name monitoreo -p 8002:80 -v /var/www/monitoreo/:/usr/local/apache2/htdocs/ httpd:2.4'
        sh 'virtualenv venv && . venv/bin/activate && pip3 install -r requirements.txt'
        sh 'python3 app.py'
        echo 'deploying the application'
      }
    }

  }
}