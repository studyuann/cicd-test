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
                 sshagent(credentials:['Deploy-Privatekey']) {
                    // 원격 서버의 /var/www/html/ 권한이 ubuntu에게 있는지 확인이 필요할 수 있습니다.
                    sh "scp -o StrictHostKeyChecking=no index.html ubuntu@3.35.149.250:/var/www/html/"
                 }
            }
        }
    }
}