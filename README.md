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
./network.sh deployCC -ccn basic -ccp ../asset-transfer-basic/chaincode-go -ccl go
git clone https://github.com/hyperledger/fabric-sdk-java.git

apt install docker.io
apt install docker-compose
git config --global core.autocrlf false
git config --global core.longpaths true

//Metamask
//migrations/2_deploy_contracts.js
const MessageStorage = artifacts.require("MessageStorage");

module.exports = function (deployer) {
  deployer.deploy(MessageStorage);
};

mkdir truffle-ganache-dapp
cd truffle-ganache-dapp
truffle init

//npm install dotenv
//contracts/MessageStorage.sol
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.18;

contract MessageStorage {
    string private message;

    function setMessage(string memory _message) public {
        message = _message;
    }

    function getMessage() public view returns (string memory) {
        return message;
    }
}

truffle migrate --reset --network development

//cofig.js
module.exports = {
  networks: {
    development: {
      host: "127.0.0.1",
      port: 7545,
      network_id: "5777", // Use Ganache network ID
    }
  },
  compilers: {
    solc: {
      version: "0.8.18"
    }
  }
};

ganache --port 7545 --networkId 5777
//Copy truffle/build/contracts/MessageStorage.json → frontend/src/contracts/.
cd frontend
npm start
import React, { useState, useEffect } from "react";
import { ethers } from "ethers";
import MessageStorageABI from "./contracts/MessageStorage.json";

const CONTRACT_ADDRESS = "PASTE_YOUR_CONTRACT_ADDRESS_HERE";

function App() {
  const [message, setMessage] = useState("");
  const [storedMessage, setStoredMessage] = useState("");

  useEffect(() => {
    fetchMessage();
  }, []);

  async function fetchMessage() {
    if (!window.ethereum) return alert("MetaMask required!");

    const provider = new ethers.BrowserProvider(window.ethereum);
    const contract = new ethers.Contract(CONTRACT_ADDRESS, MessageStorageABI.abi, provider);
    setStoredMessage(await contract.message());
  }

  async function updateMessage() {
    if (!window.ethereum) return alert("MetaMask required!");

    const provider = new ethers.BrowserProvider(window.ethereum);
    await provider.send("eth_requestAccounts", []);
    const signer = await provider.getSigner();
    const contract = new ethers.Contract(CONTRACT_ADDRESS, MessageStorageABI.abi, signer);

    const tx = await contract.setMessage(message);
    await tx.wait();
    fetchMessage();
  }

  return (
    <div style={{ textAlign: "center", padding: "50px" }}>
      <h1>Blockchain Message Storage</h1>
      <p>Stored Message: {storedMessage}</p>
      <input type="text" value={message} onChange={(e) => setMessage(e.target.value)} />
      <button onClick={updateMessage}>Store Message</button>
    </div>
  );
}

export default App;

npx create-react-app frontend
cd frontend
npm install ethers web3
ganache
mkdir simple-dapp
cd simple-dapp
truffle init




