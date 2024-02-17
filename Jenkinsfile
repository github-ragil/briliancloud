pipeline {
agent any

stages {
stage(‘Build Docker’) {
steps {
sh 'docker build -t mraagil/briliancloud:nginx .'
}
}
}
}