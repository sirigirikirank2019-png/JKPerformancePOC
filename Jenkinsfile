pipeline {
    agent any

    environment {
        // Set Java and JMeter environment variables
        JAVA_HOME = 'C:\\Program Files\\Java\\jdk-17'  // Adjust to your actual Java path
        JMETER_HOME = 'C:\\Users\\sreek\\Desktop\\apache-jmeter-5.6.3'  // Adjust to your JMeter path
        JMETER_TEST = "${WORKSPACE}/Scripts/P01_HTTPBinAPI_PT_StreeTest.jmx"  // Adjust to your test plan path
        JMETER_RESULTS = "${WORKSPACE}/tmp/res"  // Folder for JMeter results
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
                bat """
                set PATH=%JAVA_HOME%\\bin;%PATH%  // Ensure Java is in the PATH
                cd "%JMETER_HOME%\\bin"
                jmeter -n ^
                       -t "%JMETER_TEST%" ^
                       -l "%JMETER_RESULTS%\\results_%BUILD_NUMBER%.jtl" ^
                       -e ^
                       -o "%JMETER_RESULTS%\\report_%BUILD_NUMBER%"
                """
            }
        }

        stage('Archive Results') {
            steps {
                // Archive the JMeter results after the test run
                archiveArtifacts artifacts: '**/tmp/res/**', allowEmptyArchive: true
            }
        }
    }

    post {
        success {
            echo 'Performance test PASSED'
        }
        failure {
            echo 'Performance test FAILED'
        }
    }
}
