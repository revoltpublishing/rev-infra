# Use the official Jenkins LTS image with JDK 11
FROM jenkins/jenkins:lts-jdk11

# Switch to root user to install Docker CLI and other tools
USER root

# Update package list and install dependencies
RUN apt-get update && apt-get install -y \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg \
    lsb-release \
    && curl -fsSL https://download.docker.com/linux/debian/gpg | apt-key add - \
    && echo "deb [arch=$(dpkg --print-architecture)] https://download.docker.com/linux/debian $(lsb_release -cs) stable" \
    | tee /etc/apt/sources.list.d/docker.list > /dev/null \
    && apt-get update \
    && apt-get install -y docker-ce-cli

# Expose Jenkins default port (8080) and agent connection port (50000)
RUN jenkins-plugin-cli --plugins "blueocean docker-workflow token-macro json-path-api"
EXPOSE 8080 50000

# Optionally, expose additional custom port (15432)
EXPOSE 15432

# Switch back to the Jenkins user
USER jenkins

# Install Jenkins plugins (e.g., Blue Ocean and Docker Workflow)
RUN jenkins-plugin-cli --plugins "blueocean docker-workflow"

# Environment variable for Jenkins home
ENV JENKINS_HOME /var/jenkins_home

# docker run -d -p 8080:8080 -p 50000:50000 