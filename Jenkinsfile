node
{
    def mavenHome = tool name: "maven 3.6.2"
    def buildNumber = BUILD_NUMBER
    stage('CheckoutCode')
    {
        git credentialsId: '33b14771-b858-4cc9-aac8-40a58e2ee672', url: 'https://github.com/navya-devops-applications/docker.git'
    }
    stage('Build'){
        sh "${mavenHome}/bin/mvn clean package"
    }
    stage('CreatingdockerImage')
    {
        sh "docker build -t navyadevops/docker:${buildNumber} ."
    }
    stage('Authenticating')
    {
    withCredentials([string(credentialsId: 'Docker_hub_pwd', variable: 'Docker_hub_pwd')]) 
    {
    sh " docker login -u navyadevops -p ${Docker_hub_pwd} "
    }
    }
    stage('Pushimage')
    {
        sh "docker push navyadevops/docker:${buildNumber}"
    }
    stage('pullimage')
    {
        sh "docker pull navyadevops/docker:${buildNumber}"
    }
    stage('creating container and Deploying')
    {
    sshagent(['5c4106cc-e4bc-498d-ae31-bf48272070d0']) 
    {
        sh "ssh -o StrictHostKeyChecking=no ubuntu@13.233.201.85 docker rm -f java-web-appcontainer || true"
        sh "ssh -o StrictHostKeyChecking=no ubuntu@13.233.201.85 docker run -d -p 8080:8080 --name java-web-appcontainer navyadevops/docker:${buildNumber}"
    }
    }
}
