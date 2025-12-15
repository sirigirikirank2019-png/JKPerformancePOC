pipeline {
    agent any

    environment {
        JAVA_HOME = 'C:\\Program Files\\Java\\jdk-17' //C:\Program Files\Java\jdk-17
        JMETER_HOME = 'C:\\Users\\sreek\\Desktop\\apache-jmeter-5.6.3'  // Update with your actual JMeter installation path
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
                set JAVA_HOME=C:\\Program Files\\Java\\jdk-17  // Ensure JMeter uses the correct Java version
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
