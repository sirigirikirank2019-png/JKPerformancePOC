pipeline {
    agent any

    environment {
        JAVA_HOME = 'C:\\Program Files\\Java\\jdk-17'
        JMETER_HOME = 'C:\\Users\\sreek\\Desktop\\apache-jmeter-5.6.3'
        JMETER_TEST = "${WORKSPACE}/Scripts/P01_HTTPBinAPI_PT_StreeTest.jmx"
        JMETER_RESULTS = "${WORKSPACE}/tmp/res"
    }

    stages {
        stage('Checkout Code') {
            steps {
                // Checkout code from Git repository
                git branch: 'main', url: 'https://github.com/sirigirikirank2019-png/JKPerformancePOC.git'
            }
        }

        stage('Verify Java Version') {
            steps {
                echo "JAVA_HOME is: ${env.JAVA_HOME}"
                bat """
                set PATH=%JAVA_HOME%\\bin;%PATH%
                java -version
                """
            }
        }

        stage('Run JMeter Test') {
            steps {
                echo "Running JMeter Test..."

                // Ensure the results directory exists
                bat 'if not exist "%JMETER_RESULTS%" mkdir "%JMETER_RESULTS%"'

                bat """
                set PATH=%JAVA_HOME%\\bin;%PATH%
                echo "JAVA_HOME is set to: %JAVA_HOME%"
                echo "JMETER_HOME is set to: %JMETER_HOME%"
                cd "%JMETER_HOME%\\bin"
                jmeter -n ^
                       -t "%JMETER_TEST%" ^
                       -l "%JMETER_RESULTS%\\results_%BUILD_NUMBER%.jtl" ^
                       -e ^
                       -o "%JMETER_RESULTS%\\report_%BUILD_NUMBER%" 2>&1 1>jmeter_output.log
                """
            }
        }

        stage('Archive Results') {
            steps {
                // Archive JMeter result files
                archiveArtifacts artifacts: 'tmp/res/**/*.jtl', allowEmptyArchive: true
                // Archive JMeter output log
                archiveArtifacts artifacts: 'jmeter_output.log', allowEmptyArchive: true
            }
        }
    }

    post {
        success {
            echo 'Performance test PASSED'
        }
        failure {
            echo 'Performance test FAILED'
            // Archive the log file in case of failure
            archiveArtifacts artifacts: 'jmeter_output.log', allowEmptyArchive: true
        }
    }
}
