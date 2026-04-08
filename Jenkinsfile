pipeline {
    agent any

    stages {
        stage('Github Pull') {
            steps {
                git branch:'main',url:'https://github.com/studyuann/cicd-test.git'
            }
        }        
        stage('Git clone end') {
            steps {
               sh 'touch cicd_test.txt'
               sh 'echo "git clone end" > cicd_test.txt'
            }
        }
        
    }
}
