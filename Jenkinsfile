pipeline {
    agent any 
    stages {
        stage('Setup Python') {
            steps {
                sh '''
                    apt-get update
                    apt-get install -y python3 python3-pip
                '''
            }
        }
        stage('Build') { 
            steps {
                sh 'python -m py_compile sources/add2vals.py sources/calc.py' 
                stash(name: 'compiled-results', includes: 'sources/*.py*') 
            }
        }
    }
}