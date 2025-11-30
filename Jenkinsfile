pipeline {
    // Force pipeline to run only on your WSL agent
    agent { label 'Slave-01' }

    stages {
        // Stage 1.8: Build and Package (no Nexus, no Sonar yet)
        stage('Build & Package') {
            steps {
                sh 'mvn clean package'
            }
        }

        // OPTIONAL: simple “deployment” just to mimic running the app
        stage('Deploy to Staging') {
            steps {
                echo 'Deploying application to staging environment...'
                // Maven replaces the original jar with the shaded one,
                // so java-webapp-1.0.jar is the fat jar.
                sh 'java -jar target/java-webapp-1.0.jar &'
                echo 'Deployment successful! Application should be running.'
            }
        }
    }
}


