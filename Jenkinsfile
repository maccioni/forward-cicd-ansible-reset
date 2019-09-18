pipeline {
    agent any

    stages {
        stage('Rollback panos configurations') {
            steps {
                slackSend (message: "STARTED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.JENKINS_URL}/blue/organizations/jenkins/forward-cicd-ansible-reset/detail/master/${env.BUILD_NUMBER})",  username: 'fabriziomaccioni', token: "${env.SLACK_TOKEN}", teamDomain: 'fwd-net', channel: 'demo-notifications')
                echo "Downloaded code from https://github.com/maccioni/forward-cicd-ansible"
                sh 'env'
                sh "ssh root@10.128.2.244 'ansible-playbook rollback_changes.yml -vvvv'"
            }
        }
    }
    post {
        always {
            echo "(Post always) currentBuild.currentResult: ${currentBuild.currentResult}"
            echo "(Post always) currentBuild.Result: ${currentBuild.result}"
        }
        success {
            echo "(Post success) Pipeline executed successfully!"
            slackSend (message: "SUCCESSFUL: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.JENKINS_URL}/blue/organizations/jenkins/forward-cicd-ansible-reset/detail/master/${env.BUILD_NUMBER})", color: '#00FF00', username: 'fabriziomaccioni', token: "${env.SLACK_TOKEN}", teamDomain: 'fwd-net', channel: 'demo-notifications')
        }
        unstable {
            echo "(Post unstable) Pipeline is unstable :/"
        }
        failure {
            echo "(Post failure) Pipeline Failed!!!"
            slackSend (message: "FAILED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.JENKINS_URL}/blue/organizations/jenkins/forward-cicd-ansible-reset/detail/master/${env.BUILD_NUMBER})", color: '#FF0000', username: 'fabriziomaccioni', token: "${env.SLACK_TOKEN}", teamDomain: 'fwd-net', channel: 'demo-notifications')
        }
        changed {
            echo "(Post failure) Something changed..."
        }
    }
}
