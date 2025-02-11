# web3
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.1/install.sh | bash
nvm use system
source ~/.bashrc
nvm install 18
apt update
apt install openjdk-11-jdk -y
apt install golang-go
mkdir -p $Home/go/src/github.com/Junaid2003
cd go/src/github.com/Junaid2003
curl -sSLO https://raw.githubusercontent.com/hyperledger/fabric/main/scripts/install-fabric.sh && chmod +x install-fabric.sh
ll
./install-fabric.sh docker samples binary
ll
cd fabric-samples/test-network/fabric-sdk-java
./network.sh up createChannel -c mychannel -ca
./network.sh deployCC -ccn basic-ccp ../asset-transfer-basic/chaincode-java --ccl java
git clone https://github.com/hyperledger/fabric-sdk-java.git

