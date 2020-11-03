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
				gitcredentialsId: ${repositoryCredentials}
					url: ${repository}
			}		
		}
		stage('tests') {
			steps {
				bat 'python3 -m pytest tests/'
			}
		}
		stage ('create docker image') {
			steps {
				bat 'docker build -t cdn-dns-controller -f docker/Dockerfile .'
			}
		}
		stage ('run docker') {
			steps {
				bat 'docker run -it -d -p 5000:5000 --rm -neme cdn-dns-controller cdn-dns-controller'
			}
		}
		stage ('sanity test') {
			steps {
				bat 'curl locahost:5000/'
			}
		}
		stage ('cleanup') {
			steps {
				bat 'docker rm -f cdn-dns-controller'
			}
		}
	}
}
