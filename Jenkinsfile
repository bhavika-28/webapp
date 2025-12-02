pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                // Compile + package the app
                bat 'mvn -B clean package'
            }
        }

        stage('Deploy to Nexus') {
            steps {
                // This will automatically use C:\Users\DELL\.m2\settings.xml
                bat 'mvn -B deploy'

                // If you ever want to be explicit:
                // bat 'mvn -B -s C:\\Users\\DELL\\.m2\\settings.xml deploy'
            }
        }
    }
}

