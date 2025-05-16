pipeline {
    agent any
    stages {
        
stage('Build') {
    steps {
        script {
            // Run the build command, capture all output (even if it fails)
           // def buildResult = sh(script: 'mvn clean || true', returnStdout: true).trim()
           def buildResult = bat(script: 'mvn clean || exit /b 0', returnStdout: true).trim()

                    // Optionally, you can use buildResult to analyze the output
                    echo "Maven clean result: ${buildResult}" 
            // Save output to a file for record
            writeFile file: 'build_output.txt', text: buildResult
            
            // Scan for error lines (case-insensitive)
            def errorLines = buildResult.readLines().findAll { it.toLowerCase().contains('error') }
            
            if (errorLines) {
                echo "Errors found in build output:"
                errorLines.each { echo it }
                error("Build failed due to errors detected in output.")
            } else {
                echo "No errors detected in build output."
            }
        }
    }
}
        stage('Run and Capture Errors') {
            steps {
                script {
                    // Read output captured earlier
                    def output = readFile('build_output.txt')
                    def errorLines = output.readLines().findAll { it.toLowerCase().contains('error') }
                    if (errorLines) {
                        echo "Found error lines:"
                        errorLines.each { echo it }
                        error("Build failed due to errors detected in output.")
                    } else {
                        echo "No errors detected in build output."
                    }
                }
            }
        }

        
        }
}   
