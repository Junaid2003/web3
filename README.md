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

//Asset Transfer

// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;
contract AssetTransfer {
    struct Asset {
        uint256 id;
        string name;
        address owner;
    }

    mapping(uint256 => Asset) public assets;
    uint256 public assetCounter;

    event AssetRegistered(uint256 assetId, string name, address owner);
    event AssetTransferred(uint256 assetId, address from, address to);

    function registerAsset(string memory _name) public {
        assetCounter++;
        assets[assetCounter] = Asset(assetCounter, _name, msg.sender);
        emit AssetRegistered(assetCounter, _name, msg.sender);
    }

    function transferAsset(uint256 _assetId, address _newOwner) public {
        require(assets[_assetId].owner == msg.sender, "Only the owner can transfer this asset");
        require(_newOwner != address(0), "Invalid address");

        assets[_assetId].owner = _newOwner;
        emit AssetTransferred(_assetId, msg.sender, _newOwner);
    }

    function getAsset(uint256 _assetId) public view returns (string memory, address) {
        require(assets[_assetId].owner != address(0), "Asset does not exist");
        return (assets[_assetId].name, assets[_assetId].owner);
    }
}
//Asset Transfer

//Voting
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract Vote{

    struct Voter {
        bool hasVoted;
        uint256 votedProposalId;
    }

    struct Candidate {
        string name;
        uint256 voteCount;
    }

    address adminAddr;
    uint startTime;
    uint endTime;
    mapping(address => Voter)  voters;
    Candidate[]  proposals;
    mapping (string => uint) candidate_votes;
    event Voted(address indexed voter, uint256 indexed proposalId);
    event ProposalAdded(uint256 indexed proposalId, string name);

    constructor(){
        adminAddr = msg.sender;
    }

    modifier adminAccess(){
        require(msg.sender == adminAddr,"Not an admin");
        _;
    }

    modifier checkTime(){
        require(startTime < block.timestamp && endTime > block.timestamp ,"Out of time");
        _;
    }

    modifier resultTime(){
        require(endTime < block.timestamp ,"Voting is ongoing");
        _;
    }

    modifier hasNotVoted() {
        require(!voters[msg.sender].hasVoted, "You have already voted");
        _;
    }



    modifier validProposal(uint256 proposalId) {
        require(proposalId < proposals.length, "Invalid proposal ID");
        _;
    }

    function createVoterList(string memory _name1, string memory _name2,uint256 _votingDurationMinutes) public adminAccess {
        proposals.push(Candidate({name: _name1, voteCount: 0}));
        proposals.push(Candidate({name: _name2, voteCount: 0}));
        startTime = block.timestamp;
        endTime = startTime + (_votingDurationMinutes * 1 minutes);
    }

    function vote(uint256 proposalId) external hasNotVoted checkTime validProposal(proposalId) {
        voters[msg.sender].hasVoted = true;
        voters[msg.sender].votedProposalId = proposalId;
        proposals[proposalId].voteCount++;
        emit Voted(msg.sender, proposalId);
    }

    function getProposal(uint256 proposalId) external view validProposal(proposalId) returns (string memory) {
        Candidate memory proposal = proposals[proposalId];
        return (proposal.name);
    }

    function hasVotingEnded() public view returns (bool) {
        return block.timestamp > endTime;
    }

    function Winner() public view returns (string memory, uint256) {
        require(block.timestamp > endTime, "Voting is not over yet.");
        uint256 winningVoteCount = 0;
        string memory winner = "";
        bool isTie = false;
        for (uint256 i = 0; i < proposals.length; i++) {
        Candidate memory candidate = proposals[i];

            if (candidate.voteCount > winningVoteCount) {
                winningVoteCount = candidate.voteCount;
                winner = candidate.name;
                isTie = false;
            } else if (candidate.voteCount == winningVoteCount && bytes(winner).length != 0) {
                isTie = true;
            }
        }
            if (isTie) {
            return ("There is a tie between the candidates!", winningVoteCount);
        }
        return (winner, winningVoteCount);
    }
}
//Voting

//car auction
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract CarAuction {
    address public owner;
    address public highestBidder;
    uint public highestBid;
    bool public auctionEnded;

    struct Car{
    string  carModel;
    string  carCondition;
    uint  carYear;
    uint _startingBid;
    }
    Car public car;
    event NewBid(address indexed bidder, uint amount);
    event AuctionEnded(address winner, uint amount);
    event Withdrawn(address indexed bidder, uint amount);  


    constructor (
        string memory _carModel,
        string memory _carCondition,
        uint _carYear,
        uint _startingBid
    ) {
        owner = msg.sender;
        car = Car(_carModel, _carCondition, _carYear, _startingBid);
        auctionEnded = false;
    }

    // Function to place a bid
    function placeBid() external payable {
        require(!auctionEnded, "Auction has already ended.");
        require(msg.value > highestBid, "Bid must be higher than the current highest bid.");

        // Refund the previous highest bidder
        if (highestBidder != address(0)) {
            payable(highestBidder).transfer(highestBid);
        }

        highestBidder = msg.sender;
        highestBid = msg.value;
        emit NewBid(msg.sender, msg.value);
    }

    // Function to end the auction
    function endAuction() external {
        require(msg.sender == owner, "Only the owner can end the auction.");
        require(!auctionEnded, "Auction has already ended.");
        auctionEnded = true;
        emit AuctionEnded(highestBidder, highestBid);
    }

   // Function to withdraw funds for the highest bidder after auction ends
    function withdraw() external {
        require(auctionEnded, "Auction has not ended yet.");
        require(msg.sender == owner, "Only the owner can withdraw.");

        uint amount = highestBid;
        require(amount > 0, "No funds to withdraw.");  // Prevent withdrawing zero funds
        require(address(this).balance>=amount,"Insufficient contract balance");

        highestBid = 0;  // Prevent re-entrancy attacks
        payable(owner).transfer(amount);

        emit Withdrawn(owner, amount);  // Emit event after successful transfer

    }
    function contractBalance()public view returns(uint){
        return address(this).balance;
    }
}
//car auction

//global finance
//SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract FinancialContract {
    address public owner;
    mapping(address => uint256) public etherBalances;
    mapping(address => uint256) public assetBalances;

    constructor(uint balance) {
        owner = msg.sender;
        etherBalances[msg.sender] += balance;
    }
    modifier onlyOwner() {
        require(
            msg.sender == owner,
            "Only the owner can perform this operation."
        );
        _;
    }
    function transferFunds(address payable to, uint256 amount) public {
        require(msg.sender != to, "You cannot send funds to yourself.");
        require(etherBalances[msg.sender] >= amount, "Insufficient balance.");
        etherBalances[to] += amount;
        etherBalances[msg.sender] -= amount;
    }
    function issueAsset(address recipient, uint256 amount) public onlyOwner {
        assetBalances[recipient] += amount;
    }
    function transferAsset(address to, uint256 amount) public {
        require(assetBalances[msg.sender] >= amount, "Insufficient assets.");
        assetBalances[msg.sender] -= amount;
        assetBalances[to] += amount;
    }
    receive() external payable {
        etherBalances[msg.sender] += msg.value;
    }
}
//global finance

//fitness reward
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract fitness {
    struct activity {
        address userid;
        string name ;
        uint points;
        uint time;
    }

    activity[] activity_array;
    mapping(string => uint) public act_score;
    mapping(uint => activity) public activity_list;
    uint a_points = 0;
    event New_activity(address useraddress,string activityname);

    function register_activity(string memory name, uint score) public returns(string memory){
        act_score[name] = score;
        return "Activity Registered";
    }

    function add_activity(string memory a_name,uint a_time)public returns(string memory) {
        a_points = act_score[a_name];
        activity_array.push(activity({userid: msg.sender, name:a_name, points:a_points, time:a_time}));
        uint i= activity_array.length-1;
        activity_list[i]=activity({userid: msg.sender, name:a_name, points:a_points, time:a_time});
        emit New_activity(msg.sender, a_name);
        return "added successfully";  
    }

    function getPoints(address user_id) public view returns(uint){
        uint temp = 0;
        for(uint j = 0; j<=activity_array.length-1; j++){
            address add = activity_list[j].userid;
            if(add == user_id){
                temp = temp + activity_list[j].points;
            }
        }
        return temp;
    }
}



