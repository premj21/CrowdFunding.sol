//SPDX-License-Identifier:MIT
pragma solidity ^0.8.19;
contract Crowdfunding {
    mapping (address=>uint) public contributors; 
    address owner ; 
    uint public target ; 
    uint public deadline;
    uint public COntriNum;
    uint public raisedAmmout; 
    
    constructor(uint _target,uint _deadline){
        owner = msg.sender; 
        target = _target; 
        deadline = block.timestamp+_deadline; 
    }
    function sendEth()public payable {
        require(raisedAmmout<target && deadline>block.timestamp,"Can't Contribute");
        if(contributors[msg.sender] == 0){
            COntriNum++; 
        }
        contributors[msg.sender]+=msg.value;
        raisedAmmout+=msg.value; 
    }
    function cheksendedMoney() public view  returns (uint) {
       require(contributors[msg.sender]>0,"You Havnet Contributed");
       return contributors[msg.sender];
    }
    function checktimestamp()public view returns (uint){
        if(block.timestamp>deadline) return 1; 
        return block.timestamp;
    }
    function getBackMoney() public {
      require(contributors[msg.sender]>0,"You Havnet Contributed");
      require(raisedAmmout<target && deadline<block.timestamp,"not eligible for refund");
       address payable user = payable(msg.sender);
       user.transfer(contributors[msg.sender]);
       raisedAmmout-=contributors[msg.sender]; 
       contributors[msg.sender] = 0; 
       COntriNum-=1;
    }
}
