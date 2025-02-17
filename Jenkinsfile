pipeline {
    agent any
    stages {
        stage('Check Python') {
            steps {
                sh 'which python || which python3'
                sh 'python --version || python3 --version'
            }
        }
        stage('Build') { 
            steps {
                sh 'python3 -m py_compile sources/add2vals.py sources/calc.py' 
                stash(name: 'compiled-results', includes: 'sources/*.py*') 
            }
        }
    }
}
