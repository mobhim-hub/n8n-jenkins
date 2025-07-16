pipeline {
  agent { label 'docker-builder' }

  // Optional: Only needed if you plan to use it in deployment later
  // environment {
  //   DEPLOY_KEY = credentials('jenkins-admin')
  // }
  
  triggers {
    githubPush()
  }

  stages {
    stage('Clean Workspace') {
      steps {
        cleanWs()
      }
    }

    stage('Checkout Code') {
      steps {
        git branch: 'main',
            url: 'https://github.com/mobhim-hub/jenkins-github.git',
            credentialsId: '28187223-2904-4aaa-9b87-0df8b4f0daa2'
      }
    }

    stage('Install Dependencies') {
      steps {
        script {
          if (fileExists('package.json')) {
            sh 'npm install'
          } else {
            echo '⚠️ Skipping install — no package.json found'
          }
        }
      }
    }

    stage('Run Tests') {
      steps {
        script {
          if (fileExists('package.json')) {
            sh 'npm test || true'
          } else {
            echo '⚠️ Skipping tests — no package.json found'
          }
        }
      }
    }

    stage('Build App') {
      steps {
        script {
          if (fileExists('package.json')) {
            sh 'npm run build || true'
            archiveArtifacts artifacts: 'build/**', fingerprint: true
          } else {
            echo '⚠️ Skipping build — no package.json found'
          }
        }
      }
    }
    stage('hello1'){
      steps{
        echo 'automatic run'
      }
    }

    // 🟢 Optional Deploy Stage — Uncomment if/when ready
    /*
    stage('Deploy to Remote') {
      steps {
        sshagent(['remote-server-key']) {
          sh '''
            scp -r build/* ubuntu@192.168.1.100:/opt/app/
          '''
        }
      }
    }
    */
  }

  post {
    always {
      echo '🔁 Build finished.'
    }
    success {
      echo '✅ Build succeeded!'
    }
    failure {
      echo '❌ Build failed. Check logs above.'
    }
  }
}
