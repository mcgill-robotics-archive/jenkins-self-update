# Jenkins Self Update

This repository maintains the Jenkins pipeline that automatically updates
Jenkins' plugins.

The pipeline will run daily at 8AM and will update any out-of-date plugins and
safe-restart the Jenkins instance. The only requirement is for a GitHub personal
access token (PAT) to be saved to a file at `/home/jenkins/pat_file`.
