pipeline {
    agent {
        docker { 
            image 'python:3.12-alpine'  // Using a stable Python version
        }
    }
    stages {
        stage('Build') { 
            steps {
                sh 'python -m py_compile sources/add2vals.py sources/calc.py' 
                stash(name: 'compiled-results', includes: 'sources/*.py*') 
            }
        }
    }
}