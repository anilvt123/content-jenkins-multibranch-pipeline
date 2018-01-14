pipeline {
 agent any
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
										sh 'java -jar rectangle.jar 4 4'
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
								 sh 'git pull origin develop'
								 echo 'Checking Out preprod'
								 sh 'git checkout preprod'
								 echo 'Merging develop into preprod'
								 sh 'git merge develop'
								 echo 'Git Push to Origin'
								 sh 'git push origin preprod'
								 }
			 }
 }
}