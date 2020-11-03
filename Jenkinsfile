pipeline {
	encironment {
		repository = "https://github.com/RandomMac520/jenkins.git" // github url
		repositoryCredentials = "" // Jenkins cred ID
	}
	agent any
	options {
		timeout(time: 1, unit: 'HOURS')
	}
	stages {
		stage ('git clone') {
			steps {
				script {
				git gitcredentialsId: ${repositoryCredentials}
					url: ${repository}
				}
			}
		}
		stage('tests') {
			steps {
				script {
				bat 'python3 -m pytest tests/'
				}
			}
		}
		stage ('create docker image') {
			steps {
				script {
				bat 'docker build -t cdn-dns-controller -f docker/Dockerfile .'
				}
			}
		}
		stage ('run docker') {
			steps {
				script {
				bat 'docker run -it -d -p 5000:5000 --rm -neme cdn-dns-controller cdn-dns-controller'
				}
			}
		}
		stage ('sanity test') {
			steps {
				script {
				bat 'curl locahost:5000/'
				}
			}
		}
		stage ('cleanup') {
			steps {
				script {
				bat 'docker rm -f cdn-dns-controller'
				}
			}
		}
	}
}
