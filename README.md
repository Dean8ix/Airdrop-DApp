 
# Airdrop-DApp

Airdrop is a simple ERC1155 smart contract with a functional decentralized application (DApp) integrated with the smart contract. 
The smart contract incentivizes developers to sign on the platform and mint either an NFT token or an ERC-20 token for them

## Description
This program is a Dapp created using a javascript framework to connect to the blockchain - ethers.js and some javascript codes. On the front-end, the players can only guess the letter only when the owner have set the letter.

## Smart Contract
```javascript
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

import "@openzeppelin/contracts/token/ERC1155/ERC1155.sol";

contract Airdrop is ERC1155 {
    address owner;

    mapping(uint256 => string) tokenURI;

    mapping(address => Developers) developers;

    struct Developers {
        address user;
        bool isDev;
    }

    constructor() ERC1155("") {
        owner = msg.sender;

        _setURIS();
    }

    modifier onlyOwner() {
        require(msg.sender == owner, "Only Owner");
        _;
    }

    function signUp() external {
        require(developers[msg.sender].user == address(0), "Signed Up!");

        developers[msg.sender] = Developers({user: msg.sender, isDev: true});
    }

    function airdrop(address to, uint256 id, uint256 value) external onlyOwner {
        assert(id > 0);
        if (!developers[to].isDev) revert("Not qualified for airdrop!");
        if (id == 1 && value > 1) revert("You can only mint 1 NFT");

        super._mint(to, id, value, "");
    }

    function contractURI() public pure returns (string memory) {
        return
            "https://ipfs.io/ipfs/QmfThmmntg4dKSa8EHmbRQWUkt36fzCK54EV3BeT4Ks7hE/ventura.json";
    }

    function uri(uint256 _id) public view override returns (string memory) {
        return tokenURI[_id];
    }

    function balanceOf(
        address account,
        uint256 id
    ) public view override returns (uint256) {
        return super.balanceOf(account, id);
    }

    function _setURIS() private {
        tokenURI[
            1
        ] = "https://ipfs.io/ipfs/QmfThmmntg4dKSa8EHmbRQWUkt36fzCK54EV3BeT4Ks7hE/creator.json";

        tokenURI[
            2
        ] = "https://ipfs.io/ipfs/QmfThmmntg4dKSa8EHmbRQWUkt36fzCK54EV3BeT4Ks7hE/poap.json";
    }
}
```

# Getting Started

## Installing

- Clone the project by typing ```git clone https://github.com/Dean8ix/Airdrop-DApp/``` in your terminal.
- After cloning the project, ```cd``` into the project and type ```npm i``` to install all dependencies for the project
- Deploy your contract on your preferred chain, preferably on Avalanche. Set up your hardhat.config file to be able to deploy on your preferred network. run ```npx hardhat run scripts/deploy.js --network <YOUR_NETWORK>``` to deploy
- Once your contract is deployed, copy the contract address and head to index.js, add the contract address here
  ```javascript
  const contractAddress = "0x5FbDB2315678afecb367f032d93F642f64180343";
  ```
- When the contract address is added, run ```npm run dev``` to start your frontend
- Once the front-end is up, interact with the contract.

## Authors

Michael Dean

## License
This project is licensed under the MIT License - see the LICENSE.md file for details
