pipeline {
	agent any
		stages {
			stage('Clone Repo') {
				steps { git branch: 'main', url: 'https://github.com/venkatjp-sam2/jp-sterling.git' }
					}				
		stage('Build Docker Image') {
				steps { sh 'docker build -t venkatjp-sam2/myapp:$BUILD_NUMBER .' }
					}
			stage('Push Image') {
				steps {
					withCredentials([string(credentialsId: '@Govinda2050@', variable: 'DOCKER_PASS')]) {
					sh "echo $DOCKER_PASS | docker login -u pavanij2005 --password-stdin"
					sh "docker push pavanij2005/myapp:$BUILD_NUMBER"
					}
			}
		}
		stage('Deploy to Prod') {
			steps {
				sshagent(['ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQDsiAKG89csZirJOW/KHfPjZWl8LBKHUpbYKR/c1TrhXayfN5kVkQmR81PSpTWTXc6LaFYQboPc1aMkSAO54y45WG0/nXTT8PCWPrRB8nFop1Wdwfnr1nZcBEAU92eFfQk6P9ik5jyfIdNosROXuKR/GlaVpMgxU70voSKbN9Gc6hiu4saFhVK/GvsV7jVk+90q744Ywc2/NSePYAlhH7xZ+dOb1uWKGPBtbxFJxd1OndvtsnEu/DqRouaKC807RAzhcSn8BLMnjnrh9CjG9jLMAUje18+5QfDY1lYgj3XBokB7V/U2XFpC+NB9JQ88pw50YXIRdhzv/p/QvPbPJhVthTwfsTbQZpcmOYX39C6dREN3yJ6YSX4vVKXnQfFIcn6fbIvz12Tumaec4SttHSxA30zuoyV0b4GtPmkutKw3yerFK6Ph9tbUNsDAluYEViuteMFk4mU5xMPhSRJF24JENeh3V0bQMqE1uhgysf2VtEcokVb+Pyx4iqJORzeM6eY2fb8ZJdb5BZDJ1pCWZrPmVcp60sQdv+PHZJUyaPBjmhzKGNURpEBXemMp6HoEL6SrhXl7ss2LYm+AgBdfOoOjAxV9BNG4DzM5HSR3qmnzyI5mson1uxD2vnvfgPgrWb2pYkIYuBVoML1DUg5+8ucrFjwXL3RLWeADVIjj4NhBgw== jp@MacBookPro']) {
					sh '''
					ssh ubuntu@<server-ip> "docker pull <dockerhub-user>/myapp:$BUILD_NUMBER &&
					docker.stop app || true &&
					docker rm app || true &&
					docker run -d --name app -p 3000:3000 <dockerhub-user>/myapp:$BUILD_NUMBER"
					'''
					}
				}
			}
		}
	}
