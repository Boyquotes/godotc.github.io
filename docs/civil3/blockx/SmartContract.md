

## Slang

 - Solidity
> Solidity是一种用于编写智能合约的高级编程语言。Solidity的语法和结构类似于C++和JavaScript，支持面向对象编程j。
> 使用Solidity可以编写各种类型的智能合约，包括代币合约、众筹合约、去中心化交易所合约等。Solidity合约可以被编译成EVM（以太坊虚拟机）字节码，并在以太坊网络上部署和执行。

  - ERC
	 > ERC 以太坊协商意见请求（Ethereum Request for Comments）的缩写 ,ERC是以太坊请求代币标准（Ethereum Request for Comment），其中包括了各种以太坊代币的规范，以便于不同代币的开发者遵循同样的标准进行代币的开发，这样就可以更容易地与其他以太坊代币进行交互。目前，ERC-20是最广泛应用的代币标准之一，它定义了一些必需的函数，如代币的名称、总量、余额查询和转账等。
  
- EIP
	> EIP是以太坊改进提案（Ethereum Improvement Proposal），它们提出了以太坊协议的新特性或更改，并经过社区的讨论和审查后被采纳或者被放弃。EIPs不仅仅用于提出协议变更，还可以用于建议新的标准和规范，例如ERC-20就是以太坊中的一个EIP。
 
---

## 智能合约是啥?

- 写好的代码与逻辑
- Transparent
- Immutable
- Just get/set  (分布式系统: 效率与数据一致性不可兼得)

## Test Net
- Own node (geth)
- Ganache

## Load

- 效率是非常低的
- 处理交易需要广播并同步，消耗gas
- 而查询则不需要，没有修改链上的数据，每个节点都可以进行


---

## The standard interfaces  of ERC-20, ERC-721

- ERC20  Token
```sol
// Query
function totalSupply() public view returns (uint256);
function balanceOf(address account) public view returns (uint256);

// Transaction
function transfer(address recipient, uint256 amount) public returns (bool);
function transferFrom(address sender, address recipient, uint256 amount) public returns (bool);

function allowance(address owner, address spender) public view returns (uint256);
function approve(address spender, uint256 amount) public returns (bool);

// Events
event Transfer(address indexed from, address indexed to, uint256 value);
event Approval(address indexed owner, address indexed spender, uint256 value);

```

---

 - ERC721 NFT
```sol
// Query
function balanceOf(address owner) public view returns (uint256 balance);
function ownerOf(uint256 tokenId) public view returns (address owner);
function tokenURI(uint256 tokenId) public view returns (string memory);

// Transaction
function transferFrom(address from, address to, uint256 tokenId) public payable;
function safeTransferFrom(address from, address to, uint256 tokenId, bytes data) public payable;
function safeTransferFrom(address from, address to, uint256 tokenId) public payable;

function approve(address to, uint256 tokenId) public payable;
function setApprovalForAll(address operator, bool _approved) public;
function getApproved(uint256 tokenId) public view returns (address operator);
function isApprovedForAll(address owner, address operator) public view returns (bool);


// Event
event Transfer(address indexed from, address indexed to, uint256 indexed tokenId);
event Approval(address indexed owner, address indexed approved, uint256 indexed tokenId);
event ApprovalForAll(address indexed owner, address indexed operator, bool approved);

```


---

# Question?

[→Back](Blocx-Index.md)
