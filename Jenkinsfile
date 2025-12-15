pipeline {
    agent any

    environment {
        JMETER_HOME = 'C:\\apache-jmeter-5.6.3'  // Update with your actual JMeter installation path
        // Set the JMeter test plan path in the Git repository
        JMETER_TEST = "${WORKSPACE}/Scripts/P01_HTTPBinAPI_PT_StreeTest.jmx"
        // Set the results directory within Jenkins workspace (tmp/res)
        JMETER_RESULTS = "${WORKSPACE}/tmp/res"  
    }

    stages {
        stage('Checkout Code') {
            steps {
                // Checkout the code from your Git repository
                git branch: 'main', url: 'https://github.com/sirigirikirank2019-png/JKPerformancePOC.git'
            }
        }

        stage('Run JMeter Test') {
            steps {
                bat """
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
                // Archive the results from the 'tmp/res' folder within Jenkins workspace
                archiveArtifacts artifacts: 'tmp/res/**', allowEmptyArchive: true
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
