@Library('jenkins-shared-lib') _

cicdPipeline(
    agent: 'jenkins-agent',
    repo: 'https://github.com/sahukalu/django-notes-app.git',
    branch: 'main',
    dockerRepo: 'kalusahu902/notes-app-jenkins',
    dockerCreds: 'dockerhub-creds'
)
