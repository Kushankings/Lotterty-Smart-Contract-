# Lotterty-Smart-Contract-

# Functionality

Basic Function is that there is manager and participants,So participants Apply for lottery
buy giving a fixed amount amount of ether, after that manager can Roll out the Lottery and winner gets all the amount deposist.

# language
Solidity

# Software 
Remix IDE

# Code
***

// SPDX-License-Identifier: GPL-3.0

pragma solidity >=0.5.0 <0.9.0;

contract Lottery{

    address public  manager;
    address payable[] public participants;

    constructor () {
        manager = msg.sender; // GLOBAL (SENDER MSG ADRESS WILL GO HERE )
    }

    receive() external payable
     {
       require(msg.value == 1 ether);
       participants.push(payable(msg.sender));  
     }

    function getBalance() public view returns(uint){
        require(msg.sender == manager);
        return address(this).balance;
    }

    function randombasis() public view returns(uint)
    {
      return uint(keccak256(abi.encodePacked(block.difficulty, block.timestamp, participants.length )));
    }

    function findwinner() public{

        require(msg.sender==manager);
        require(participants.length>=3);
        uint r = randombasis();
        address payable winner; 
        uint index =  r % participants.length;
            winner = participants[index];
         winner.transfer(getBalance());
         participants = new address payable[](0);
        
    }

    
}

***
