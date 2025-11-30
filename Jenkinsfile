pipeline {
    // Stage 1.5: FORCES the pipeline to run ONLY on your connected WSL agent
    agent { label 'Slave-01' } 
    
    // Configure tools globally if needed, or rely on agent PATH setup
    tools {
        // Assuming Maven is installed on your Slave-01 agent via 'sudo apt install maven'
        maven 'M3' 
    }
    
    stages {
        // The first run will implicitly handle Checkout SCM
        
        // Stage 1.8: Build and Publish Artifact to Nexus
        stage('Build & Publish to Nexus') { 
            steps {
                // 'mvn clean deploy' builds, tests, and publishes the WAR/JAR file 
                // to the Nexus repository defined in your pom.xml (Step 1.10/1.11)
                sh 'mvn clean deploy' 
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
                // Example: Run the compiled application directly on the agent for the lab
                // Use the & to run it in the background
                sh 'java -jar target/webapp-*.war &'
                echo 'Deployment successful! Application should be running.'
            }
        }
    }
}
