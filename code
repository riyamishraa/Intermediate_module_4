//SPDX-License-Identifier: MIT
pragma solidity 0.8.19;

interface IERC20{
    function total_supply() external view returns (uint);
    function balance_of(address account) external view returns(uint);
    function transfer(address recipient, uint amount) external returns (bool);


    event Transfer(address indexed from, address indexed to , uint amount);
}

contract ERC20 is IERC20 {
    address public immutable owner;
    uint public total_supply;
    mapping(address => uint ) public balance_of;

    struct Item{
        uint item_Id;
        string item_name;
        uint item_price;
    }

    mapping (uint => Item) public items;
    uint public item_count;

    constructor(){
        owner= msg.sender;
        total_supply=0;
    }
    
    modifier only_owner{
        require(msg.sender == owner , "Only the contract owner can execute this function");
        _;
    }
     string public constant name ="Degen";
     string public constant symbol= "DGN";
     uint8 public constant decimals = 0;


     function transfer(address recipient , uint amount) external returns (bool){
         require(balance_of[msg.sender] >= amount,"The balance is insufficient");

         balance_of[msg.sender] -=amount;
         balance_of[recipient] += amount;

         emit Transfer(msg.sender , recipient , amount);
         return true;

     }

     function mint(address receiver , uint amount ) external only_owner{
         balance_of[receiver] +=amount;
         total_supply +=amount;
          emit Transfer(address(0), receiver, amount);
     }

     function burn (uint amount) external {
         require(amount>0,"Amount should not be zero");
         require(balance_of[msg.sender]>= amount, "The balance is insufficient");
         balance_of[msg.sender] -=amount;

         emit Transfer(msg.sender , address(0), amount);

     }


     function add_item(string memory item_name, uint256 item_price) external only_owner{
         item_count++;
         Item memory new_item = Item(item_count , item_name , item_price);
         items[item_count] = new_item;
     }

     function get_items() external view returns (Item[] memory){
         Item[] memory all_items = new Item[](item_count);

         for(uint i= 1; i<= item_count ; i++){
             all_items[i-1] = items[i];
         }

         return all_items;

     }

     function redeem(uint item_Id) external {
         require (item_Id >0 && item_Id <=item_count , "Invalid item ID");
         Item memory redeemed_item = items[item_Id];

         require(balance_of[msg.sender] >= redeemed_item.item_price, " Insufficient balance to redeem ");

         balance_of[msg.sender] -= redeemed_item.item_price;
         balance_of[owner] += redeemed_item.item_price;
         
         emit Transfer (msg.sender, address(0),redeemed_item.item_price);
     }
}
