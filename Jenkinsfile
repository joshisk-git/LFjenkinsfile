
node {

    def APP_VERSION = "1.0.0"

    try {

        stage('Checkout') {
            echo 'Cloning repository...'
            checkout scm
        }

        stage('Build') {
            echo "Building version ${APP_VERSION}"
            sh 'chmod +x app.sh'
            sh './app.sh'
        }

        stage('Test') {
            echo 'Running tests...'
            sh 'chmod +x test.sh'
            sh './test.sh'
        }

        stage('Archive') {
            echo 'Archiving artifacts...'
            archiveArtifacts artifacts: 'app.sh', fingerprint: true
        }

        echo "Build #${env.BUILD_NUMBER} succeeded!"

    } catch (Exception e) {

        echo "Build #${env.BUILD_NUMBER} failed. Check console."
        throw e

    } finally {

  
	stage('Notify Discord') {
    withCredentials([string(credentialsId: 'DISCORD_WEBHOOK', variable: 'DISCORD_WEBHOOK_URL')]) {
        script {
    def status = currentBuild.currentResult

    def color   = (status == "SUCCESS") ? 3066993 : 15158332
    def emoji   = (status == "SUCCESS") ? "✅" : "❌"
    def title   = (status == "SUCCESS") ? "Build Succeeded" : "Build Failed"

    def jobName  = env.JOB_NAME
    def buildNo  = env.BUILD_NUMBER
    def buildUrl = env.BUILD_URL

    def repo     = env.GIT_URL ?: "N/A"
    def branch   = env.GIT_BRANCH ?: "N/A"
    def commit   = env.GIT_COMMIT?.take(7) ?: "N/A"

    // get commit message
    def commitMsg = sh(script: "git log -1 --pretty=%B", returnStdout: true).trim()

    def payload = """
{
  "embeds": [{
    "title": "${emoji} ${title}",
    "description": "**Job:** ${jobName}\\n**Build:** #${buildNo}\\n**Version:** ${APP_VERSION}\\n**Status:** ${status}\\n\\n📦 **Repo:** ${repo}\\n🌿 **Branch:** ${branch}\\n🔢 **Commit:** ${commit}\\n📝 **Message:** ${commitMsg}\\n\\n[View Build Logs](${buildUrl})",
    "color": ${color},
    "footer": {
      "text": "Jenkins CI"
    }
  }]
}
"""

            sh """
                curl -s -X POST \\
                  -H "Content-Type: application/json" \\
                  -d '${payload.trim()}' \\
                  "\${DISCORD_WEBHOOK_URL}"
            """
        }
    }
}

      cleanWs()
    }
}
