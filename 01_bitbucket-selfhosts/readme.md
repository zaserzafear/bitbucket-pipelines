# Install Docker
curl https://get.docker.com | sh
sudo usermod -aG docker $USER
sudo systemctl enable docker
sudo systemctl start docker
systemctl status docker

# Add args run docker
-d --restart always

docker container run -d --restart always -it -v /tmp:/tmp -v /var/run/docker.sock:/var/run/docker.sock -v /var/lib/docker/containers:/var/lib/docker/containers:ro -e ACCOUNT_UUID={303e7c88-f193-4329-8857-eb3f15350f72}  -e RUNNER_UUID={3d54f97d-f36e-5044-b5f1-53267d7d63b8} -e RUNTIME_PREREQUISITES_ENABLED=true -e OAUTH_CLIENT_ID=Bej8SltEhZqFLnwsJoSFGFN16VUmvpLH -e OAUTH_CLIENT_SECRET=OdKZ0tGk21U0lhVJWCmsSnU5m8pBObIe8cFNdOV0NIYiIsdO1bU78IMktGoEPo3N -e WORKING_DIRECTORY=/tmp --name runner-3d54f97d-f36e-5044-b5f1-53267d7d63b8 docker-public.packages.atlassian.com/sox/atlassian/bitbucket-pipelines-runner:1