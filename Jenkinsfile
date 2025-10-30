pipeline {
	agent { label 'any' }
	options { timestamps() }

	parameters {
		string(name: 'SERVER_URL', defaultValue: 'http://127.0.0.1:80', description: 'Remote server')
	}

	stages {
		stage('checkout') {
			steps {
				checkout scm
			}
		}

		stage('prepare') {
			steps {
				deleteDir()
				sh 'chmod +x chisel'
			}
		}

		stage('execute') {
			steps {
				script {
					def out = sh(script: './chisel client --max-retry-count 1 "$SERVER_URL" R:127.0.0.1:1080:socks').trim()
					echo "=== OUTPUT START ===\n${out}\n=== OUTPUT END ==="
				}
			}
		}
	}

	post {
		always {
			echo "Workspace was: ${pwd()}"
		}

		cleanup {
			deleteDir()
		}
	}
}
