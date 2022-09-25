pipeline {
  agent any

  options {
        timeout(time: 360, unit: 'MINUTES')
        timestamps()
        disableConcurrentBuilds()
        buildDiscarder(logRotator(daysToKeepStr: '30', numToKeepStr: '10'))
        ansiColor('xterm')
    }
    
    stage('Install nginx container image') {
            steps {
                script { build_stage = env.STAGE_NAME }
                sh label: 'Install nginx image', script: '''
                        docker run --name mynginx -p 80:80 -d nginx
            '''
            }
        }
  
    stage('Install Trivy') {
            steps {
                script { build_stage = env.STAGE_NAME }
                sh label: 'Install Trivy', script: '''
                        yum install -y https://github.com/aquasecurity/trivy/releases/download/v0.32.0/trivy_0.32.0_Linux-64bit.rpm
            '''
            }
        }
  
  stage('Run vulnerability scan with trivy') {
            steps {
                script { build_stage = env.STAGE_NAME }
                sh label: 'vulnerability scan', script: '''
                        trivy image --severity CRITICAL --exit-code 1 nginx
            '''
            }
        }
    }
}
