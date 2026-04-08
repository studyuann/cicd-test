pipeline {
    agent any

    stages {
        stage('Github Pull') {
            steps {
                git branch: 'main', url: 'https://github.com/sunnykid/cicd-test.git'
            }
        }

        stage('Git clone end') {
            steps {
                sh 'touch cicd_test.txt'
                sh 'echo "git clone end22" > cicd_test.txt'
            }
        }
      stage('Deploy Server') {
    steps {
        sshagent(credentials: ['Deploy-Privatekey']) {
            // 1. 우선 권한이 있는 홈 디렉토리로 파일을 보냅니다.
            sh "scp -o StrictHostKeyChecking=no index.html ubuntu@43.203.250.150:/home/ubuntu/"
            // 2. SSH로 접속하여 sudo 권한으로 파일을 웹 서버 경로로 복사합니다.
            // 주의: 서버의 ubuntu 계정이 'sudoers'에서 NOPASSWD 설정이 되어 있어야 합니다.
            sh "ssh -o StrictHostKeyChecking=no ubuntu@43.203.250.150 sudo cp /home/ubuntu/index.html /var/www/html/"
                 }
            }
        }
    }
}