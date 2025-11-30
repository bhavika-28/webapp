pipeline {
    // Stage 1.5: FORCES the pipeline to run ONLY on your connected WSL agent
    agent { label 'Slave-01' } 
    
    stages {
        // Stage 1.8: Build and Package (no deploy to Nexus yet)
        stage('Build & Package') {
            steps {
                sh 'mvn clean package'
            }
        }

        // Stage 1.12 - 1.13: Run Static Code Analysis
        stage('Static Analysis') {
            steps {
                echo 'Starting SonarQube analysis...'
                // Assumes SonarQube server is configured in Jenkins System settings
                withSonarQubeEnv('SonarQube') { 
                    sh 'mvn sonar:sonar' 
                }
            }
        }
        
        // Stage 1.13: Quality Gate Check
        stage('Quality Gate Check') {
            steps {
                timeout(time: 5, unit: 'MINUTES') {
                    // Waits for SonarQube to return a status (PASS/FAIL)
                    waitForQualityGate abortPipeline: true 
                }
            }
        }
        
        // Stage 1.15: Final Deployment
        stage('Deploy to Staging') {
            steps {
                echo 'Deploying application to staging environment...'
                // Your Maven build is producing a JAR: java-webapp-1.0.jar
                sh 'java -jar target/java-webapp-1.0.jar &'
                echo 'Deployment successful! Application should be running.'
            }
        }
    }
}

