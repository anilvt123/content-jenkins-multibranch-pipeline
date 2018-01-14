pipeline {
 agent any
 environment {
 MAJOR_VERSION = 2
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
 sh 'java -jar rectangle.jar 7 9'
 }
 }
 stage('Promote develop to preprod') {
 when {
 branch 'develop'
 }
 steps {
 echo 'Stashing Local Changes'
 sh 'git stash'
 echo 'Checking Out develop'
 sh 'git checkout develop'
 sh 'git pull origin'
 echo 'Checking Out preprod'
 sh 'git checkout preprod'
 echo 'Merging develop into preprod'
 sh 'git merge develop'
 echo 'Git Push to Origin'
 sh 'git push origin preprod'
 }
 }
 stage('Tagging the Release') {
 when {
 branch 'preprod'
 }
 steps {
 sh "git tag RECTANGLE-${env.MAJOR_VERSION}.${BUILD_NUMBER}"
 sh "git push origin RECTANGLE-${env.MAJOR_VERSION}.${BUILD_NUMBER}"
 }
 }
 }
}