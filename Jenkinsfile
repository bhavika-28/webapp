pipeline {pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                bat 'mvn -B clean package'
            }
        }

        stage('Deploy to Nexus') {
            steps {
                // this will use C:\Users\DELL\.m2\settings.xml automatically
                bat 'mvn -B deploy'
                // if needed, you can be explicit:
                // bat 'mvn -B -s C:\\Users\\DELL\\.m2\\settings.xml deploy'
            }
        }
    }
}

