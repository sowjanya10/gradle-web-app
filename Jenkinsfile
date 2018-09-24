node {
    stage('scm checkout') {

git credentialsId: 'a57a2125-1a23-49bd-bb74-2140f4291427', url: 'https://github.com/sowjanya10/gradle-web-app.git'

}
stage('Gradle Clean Build'){
         def gradle_version = 'gradle'

withEnv( ["PATH+GRADLE=${tool gradle_version}/bin"] ){
     sh 'gradle clean build'
 }
     }
     stage('build docker image') {
    sh 'docker build -t girisala123/gradle-web .'
}
stage('push docker image') {
    withCredentials([string(credentialsId: '1a2383f6-6488-4e7a-aab0-2c4dfc2365ee', variable: 'dockerpassword')]) {
 sh 'docker login -u girisala123 -p ${dockerpassword}'
   
}
sh 'docker push girisala123/gradle-web'


}
stage('run docker image') {
    def dockerRun = ' docker run  -d -p 8080:8080 --name gradle-web-app girisala123/gradle-web'
    sshagent(['server3']) {
    sh 'ssh -o StrictHostKeyChecking=no ec2-user@172.31.20.222 docker stop gradle-web-app || true'
          sh 'ssh  ec2-user@172.31.20.222 docker rm gradle-web-app || true'
          sh 'ssh  ec2user@172.31.20.222 docker rmi -f  $(docker images -q) || true'
		  sh "ssh  ec2-user@172.31.20.222 ${dockerRun}"

}
}



}
