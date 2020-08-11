ansiColor('xterm') {
    node {
        stage('Checkout') {
            // Get some code from a GitHub repository
            checkout scm
        }
        stage('Setup') {
            sh "ansible-galaxy install -r requirements.yml"
        }
        stage('Validate') {
            sh "packer validate jenkins.json"
            sh "ansible-playbook jenkins.yml --syntax-check"
        }
        stage('Build') {
            withCredentials([usernamePassword(usernameVariable: 'AWS_ACCESS_KEY', passwordVariable: 'AWS_SECRET_KEY')]) {
            // Run the packer build
                sh "packer build -var 'aws_region=us-west-2' jenkins.json"
            }
        }
        stage('Store Artifacts') {
            archiveArtifacts 'manifest.json'
        }
    }
}
