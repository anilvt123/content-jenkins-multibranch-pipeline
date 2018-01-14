pipeline {
 agent any
 environment {
 MAJOR_VERSION = 5
 }
 stages {
 stage('build') {
 steps {
 sh 'javac -d . src/*.java'
 sh 'echo Main-Class: Rectangulator > MANIFEST.MF'
 sh 'jar -cvmf MANIFEST.MF rectangle.jar *.class'
 }
 post {
 success {
 archiveArtifacts artifacts: 'rectangle.jar', fingerprint: true
 }
 }
 }
 stage('run') {
 steps {
 sh 'java -jar rectangle.jar 9 9'
 }
 }
 stage('Promote develop to preprod') {
 when {
 branch 'develop'
 }
 steps {
 echo "Stashing Local Changes"
 sh "git stash"
 echo "Checking Out develop"
 sh 'git checkout develop'
 sh 'git pull origin'
 echo 'Checking Out preprod'
 sh 'git checkout preprod'
 echo "Merging develop into preprod"
 sh 'git merge develop'
 echo "Git Push to Origin"
 sh 'git push origin preprod'
 }
 post {
 success {
 emailext(
 subject: "${env.JOB_NAME} [${env.BUILD_NUMBER}] developPromoted to preprod",
 body: """<p>'${env.JOB_NAME} [${env.BUILD_NUMBER}]' develop Promoted to preprod":</p> 
    <p> Check console output at <a href='${env.BUILD_URL}'  > ${env.JOB_NAME} [${env.BUILD_NUMBER}]</a></p>""",  
	to: "us2000_99@yahoo.com"
 )
 }
 }
 }
 stage('Tagging the Release') {
 when {
 branch 'preprod'
 }
 steps {
 sh "git tag rectangle-${env.MAJOR_VERSION}.${BUILD_NUMBER}"
 sh "git push origin rectangle-${env.MAJOR_VERSION}.${BUILD_NUMBER}"
 }
 post {
 success {
 emailext(
 subject: "${env.JOB_NAME} [${env.BUILD_NUMBER}] NEW RELEASE",
 body: """<p>'${env.JOB_NAME} [${env.BUILD_NUMBER}]' NEW RELEASE":</p>
 <p>Check console output at <a href='${env.BUILD_URL}'> ${env.JOB_NAME} [${env.BUILD_NUMBER}] </a> </p>""",
 to: "us2000_99@yahoo.com"
 )
 }
 }
 }
 }
}