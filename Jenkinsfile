node {
    stage('Checkout SCM') {
        // Check out the source code from the SCM defined in the job configuration
        checkout scm
    }
    stage('Build') {
        // Compile the Python sources
        sh 'python3 -m py_compile sources/add2vals.py sources/calc.py'
        stash name: 'compiled-results', includes: 'sources/*.py*'
    }
    stage('Test') {
        // Run tests and generate JUnit report
        sh 'py.test --junit-xml test-reports/results.xml sources/test_calc.py'
        junit 'test-reports/results.xml'
    }
    stage('Deliver') {
        // Build the executable using PyInstaller
        sh 'pyinstaller --onefile sources/add2vals.py'
        archiveArtifacts artifacts: 'dist/add2vals', fingerprint: true
    }
}
