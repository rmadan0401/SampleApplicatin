pipeline {
    agent any

    environment {
        GRADLEW = './gradlew'
        APK_PATH = 'app/build/outputs/apk/release/app-release.apk'
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/rmadan0401/SampleApplicatin.git'
            }
        }

        stage('Setup Gradle') {
            steps {
                sh 'chmod +x gradlew'  // Make gradlew executable
            }
        }

        stage('Build APK') {
            steps {
                sh './gradlew assembleRelease'  // Build the release APK
            }
        }

        stage('Archive APK') {
            steps {
                archiveArtifacts artifacts: "${APK_PATH}", fingerprint: true
            }
        }

        stage('Upload to Play Store') {
            steps {
                googlePlayUpload apkFilesPattern: "${APK_PATH}", 
                                 googleCredentialsId: 'google-play-api', 
                                 trackName: 'beta'
            }
        }
    }

    post {
        success {
            echo 'Build and deployment successful!'
        }
        failure {
            echo 'Build or deployment failed!'
        }
    }
}
