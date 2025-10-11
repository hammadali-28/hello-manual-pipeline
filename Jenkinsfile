pipeline {
  agent any

  environment {
    IMAGE_NAME = "log-trigger-image"
    HOST_PORT  = "3090"
  }

  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }

    stage('Build Docker Image') {
      steps {
        script {
          echo "Building Docker image..."
          docker.build("${IMAGE_NAME}:${BUILD_NUMBER}")
        }
      }
    }

    stage('Run Container') {
      steps {
        script {
          echo "Running container on port ${HOST_PORT}"
          sh "docker rm -f ${IMAGE_NAME} || true"
          sh "docker run -d -p ${HOST_PORT}:80 --name ${IMAGE_NAME} ${IMAGE_NAME}:${BUILD_NUMBER}"
        }
      }
    }
  }

  post {
    success {
      script {
        emailext (
          subject: "✅ Jenkins Build #${BUILD_NUMBER} SUCCESS: ${JOB_NAME}",
          body: """
          <p>Build Successful!</p>
          <p><b>Project:</b> ${JOB_NAME}</p>
          <p><b>Build Number:</b> ${BUILD_NUMBER}</p>
          <p><b>URL:</b> <a href="${BUILD_URL}">${BUILD_URL}</a></p>
          """,
          to: "hammadsayed06655@gmail.com"
        )
      }
    }

    failure {
      script {
        emailext (
          subject: "❌ Jenkins Build #${BUILD_NUMBER} FAILED: ${JOB_NAME}",
          body: """
          <p>Build Failed!</p>
          <p><b>Project:</b> ${JOB_NAME}</p>
          <p><b>Build Number:</b> ${BUILD_NUMBER}</p>
          <p><b>URL:</b> <a href="${BUILD_URL}">${BUILD_URL}</a></p>
          """,
          to: "hammadsayed06655@gmail.com"
        )
      }
    }

    always {
      script {
        def status = currentBuild.currentResult
        def timestamp = new Date().format("yyyy-MM-dd HH:mm:ss")
        def logEntry = "${timestamp} - Build #${BUILD_NUMBER} - ${status}"
        sh "echo '${logEntry}' >> build-log.txt"
        sh "cat build-log.txt"
      }
    }
  }
}
