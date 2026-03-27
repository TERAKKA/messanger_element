# Install docker + compose
sudo apt-get install ca-certificates curl gnupg lsb-release -y
sudo mkdir -m 0755 -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
echo   "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubun
tu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin -y
docker --version
docker compose version

# Initialize synapse
cd /opt/
mkdir matrix && cd matrix
mkdir synapse nginx element
docker run -it --rm -v "$(pwd)/synapse:/data" -e SYNAPSE_SERVER_NAME=mynotesserv.ru -e SYNAPSE_REPORT_STATS=yes matrixdotorg/synapse:latest generate

# After create configs start compose
docker compose up -d
