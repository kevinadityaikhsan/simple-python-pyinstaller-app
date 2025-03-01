pipeline {
    agent any
    environment {
        EC2_USER = 'ec2-user'
        EC2_HOST = 'ec2-54-179-219-58.ap-southeast-1.compute.amazonaws.com'
    }
    stages {
        stage('Checkout SCM') {
            steps {
                checkout scm
            }
        }
        stage('Build') {
            steps {
                sh 'python3 -m py_compile sources/add2vals.py sources/calc.py'
                sh 'pyinstaller --onefile sources/add2vals.py'
                stash name: 'artifact', includes: 'dist/add2vals'
            }
        }
        stage('Test') {
            steps {
                sh 'py.test --junit-xml test-reports/results.xml sources/test_calc.py'
                junit 'test-reports/results.xml'
            }
        }
        stage('Manual Approval') {
            steps {
                input message: 'Lanjutkan ke tahap Deploy?', ok: 'Proceed'
            }
        }
        stage('Deploy') {
            steps {
                unstash 'artifact'
                
                sshagent([ 'EC2_SSH_KEY' ]) {
                    sh "scp -o StrictHostKeyChecking=no dist/add2vals ${env.EC2_USER}@${env.EC2_HOST}:/"
                    sleep 60
                }
                archiveArtifacts artifacts: 'dist/add2vals', fingerprint: true
            }
        }
    }
}
