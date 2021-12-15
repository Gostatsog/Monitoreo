pipeline {
  agent none
  stages {
    stage('Deploy') {
      agent {
        label 'master'
      }
      options {
        skipDefaultCheckout()
      }
      steps {
        sh 'pip3 install -r requirements.txt'
        sh 'pip install bokeh'
        sh 'rm -rf /var/www/monitoreo'
        sh 'mkdir /var/www/monitoreo'
        sh 'cp -Rp . /var/www/monitoreo'
        sh 'docker stop monitoreo || true && docker rm monitoreo || true'
        sh 'docker run -dit --name monitoreo -p 8002:80 -v /var/www/monitoreo/:/usr/local/apache2/htdocs/ httpd:2.4'
        echo 'deploying the application'
      }
    }

  }
}