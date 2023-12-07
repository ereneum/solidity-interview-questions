## Solidity Interview Questions - Easy

**1-** What is the difference between private, internal, public, and external functions?

 - private: Private functions can only be used internally
   and not even by derived contracts.
   
 - internal: Internal functions can only be used internally
   or by derived contracts.
   
 - public: Public functions can be used both externally and
   internally. For public state variable, Solidity automatically creates
   a getter function.
   
  - external: External functions are meant to be called by other
   contracts. They cannot be used for internal call. To call external
   function within contract this.function_name() call is required. State
   variables cannot be marked as external.

**2-** Approximately, how large can a smart contract be?

- 24,576 bytes - approximately, 24KB

**3-** What is the difference between create and create2?

- The `CREATE` and `CREATE2` opcodes differ in the parameters used to calculate the new contract’s address. The `CREATE` opcode computes the new address by taking the keccak256 hash of the sender’s address and a nonce. The `CREATE2` opcode computes the new address by taking the keccak256 hash of the constant `0xFF` (to avoid collision with the `CREATE` opcode), the sender’s address, a salt (arbitrary value provided by the sender) and the new contract’s init code. Since `CREATE2` uses a user defined parameter in it’s calculation, the new contract’s address is able to be (more reliably and flexibly) predetermined.

**4-** What major change with arithmetic happened with Solidity 0.8.0?

- Solidity 0.8.0 came with a built-in feature that automatically includes checks for arithmetic underflows and overflows. Before 0.8.0, integer underflows and overflows were allowed and would not cause an error. Since version 0.8.0, Solidity will revert if an expression causes an integer underflow or overflow. In cases where the developer is confident that there will be no arithmetic underflow or overflow, the "unchecked { your_code }" syntax allows code in curly brackets to escape this built-in check.

**5-** What special CALL is required for proxies to work?

- `DELEGATECALL` is required for proxy contracts to work because this type of call will preserve the msg (msg.sender, msg.value, etc…) and run in the context of the calling contract instead of the called contract. This enables the proxy contract to call an implementation contract to modify the proxy contract’s state. By using `DELEGATECALL`, the implementation contract can be upgraded without the smart contract system losing any information or having to change the address of the proxy.

**6-** Prior to EIP-1559, how do you calculate the dollar cost of an Ethereum transaction?

- Prior to EIP-1559, the dollar cost calculation of an Ethereum transaction was `((GAS USED * GAS PRICE / 10^-9) * CURRENT ETHER PRICE`. In this era, the miner received 100% of the gas cost. After EIP-1559, the dollar cost calculation of an Ethereum transaction is `(((BASEFEE + PRIORITY FEE) * GAS USED)) / 10^-9) * CURRENT ETHER PRICE`. Now, the miner receives a portion of the gas cost (`PRIORITY FEE`) and the other portion, the protocol fee (`BASEFEE`), is burned.

**7-** What are the challenges of creating a random number on the blockchain?

- Random number generation on the blockchain is difficult because of its deterministic and public nature. The Ethereum Virtual Machine (EVM) must produce the same output given the same input, and all data on the blockchain is public. Therefore, it is possible to determine the “random number” by copying its generation formula and reading the inputs from the blockchain.

**8-** What is the difference between a Dutch Auction and an English Auction?

- In a Dutch Auction, the auction price starts high and continuously decreases over time until a bidder submits a bid that meets or exceeds the current price, which wins the auction. In an English auction, the auction price starts low and increases over time as bidders outbid one another until the auction ends, where the highest bidder wins.

**9-** What is the difference between transfer and transferFrom in ERC20?

- In ERC20, the `transfer` function is called to transfer a token amount from `msg.sender` to a destination address. The `transferFrom` function is called to transfer a token amount from a specified source address to a destination address. For the `transferFrom` call to be successful, the source address must first give an allowance for `msg.sender` to be able to spend (at least) the transfer amount.

**10-** Which is better to use for an address allowlist: a mapping or an array? Why?

- A mapping is better for an address allowlist because it is more gas efficient. With a mapping, it is possible to check if an address is on the allowlist by directly accessing its value. Using an array, verifying an address could be costly because it would require looping through each element.

**11-** Why shouldn’t tx.origin be used for authentication?

- `tx.origin` shouldn’t be used for authentication because an attacker could execute a phishing attack and authenticate as `tx.origin`. Since `tx.origin` is the address that initiated the transaction, an attacker could influence a user to execute a malicious transaction on the attacker’s contract and then call a function to authenticate on the target contract. In this scenario, `tx.origin` is the victim’s address and the target contract would authenticate the request and allow the attacker to execute transactions as if the attacker were `tx.origin`.

**12-** What hash function does Ethereum primarily use?

- Ethereum primarily uses the `Keccak-256` hash function. This function is used for various purposes in the Ethereum network including generating addresses, forming block hashes, creating Merkle trees, generating transaction receipts, hashing within smart contracts, hashing event signatures, signature generation and verification, etc…

**13-** How much is 1 gwei of Ether?

- One gwei is equal to 0.000000001 ($1\times10^{-9}$) Ether 

**14-** How much is 1 wei of Ether?

- One wei is equal to 0.000000000000000001 Ether ($1\times10^{-18}$) Ether 

**15-** What is the difference between assert and require?

- The `assert` function should only be used to test for internal errors, and to check invariants. The `require` function should be used to ensure valid conditions, such as inputs, or contract state variables are met, or to validate return values from calls to external contracts.

**16-** What is a flash loan?

- Flash Loans allow you to borrow any available amount of assets without putting up any collateral, as long as the liquidity is returned to the protocol within one block transaction. To do a Flash Loan, you will need to build a contract that requests a Flash Loan. The contract will then need to execute the instructed steps and pay back the loan + interest and fees all within the same transaction.

**17-** What is the check-effects pattern?

- Checks Effects Interaction Pattern is a basic coding pattern that prevents an unexpected execution of a contract. This is a programming pattern that checks all the prerequisites before executing a feature in a certain function.

**18-** What is the minimum amount of Ether required to run a solo staking node?

- 32 ETH.

**19-** What is the difference between fallback and receive?

- The key distinction between `fallback` and `receive` functions in Solidity lies in their purpose and trigger conditions. The `fallback` function, present in Solidity versions prior to 0.6.0, served as a catch-all function to handle Ether transactions if a contract received Ether without matching any function signature or payable fallback function. On the other hand, the `receive` function, introduced in Solidity 0.6.0, specifically handles Ether sent to a contract without data. It is triggered when a contract receives a plain Ether transfer, offering a more explicit and dedicated way to manage Ether transactions without ambiguity, enhancing readability and control over the contract's behavior.

**20-** What is reentrancy?

- A reentrancy attack is a type of smart contract vulnerability where an exploiter contract leverages the loophole of the victim contract to continuously withdraw from it until the victim contract goes bankrupt.

**21-** As of the Shanghai upgrade, what is the gas limit per block?

- 30 million gas.

**22-** What prevents infinite loops from running forever?

- Loops in Solidity have a gas cost associated with each iteration, and if the gas limit is exceeded, the loop stops executing.

**23-** What is the difference between tx.origin and msg.sender?

- tx.origin is the address of the EOA (Externally Owned Account) that originated the transaction, and msg.sender is the address of whatever called the currently executing smart contract.

**24-** How do you send Ether to a contract that does not have payable functions, or a receive or fallback?

- An attacker can forcibly send ether to any account and this cannot be prevented. The attacker can do this by creating a contract, funding it with 1 wei, and invoking selfdestruct(victimAddress).

**25-** What is the difference between view and pure?

- View function declares that no state will be changed.  Pure function declares that no state variable will be changed or read.

**26-** What is the difference between transferFrom and safeTransferFrom in ERC721?

-   `transferFrom`: This function is used to transfer tokens from one address to another. It is a basic transfer mechanism that moves tokens from the owner's address to another specified address. However, it does not include any additional checks or validation during the transfer process. If the receiving address is a contract that doesn't support ERC721, the tokens could be lost.
    
-   `safeTransferFrom`: In contrast, `safeTransferFrom` is an enhanced version of `transferFrom` that includes additional safety checks. It first checks if the receiving address is a contract and whether that contract supports ERC721 (`ERC721Receiver`). If the receiving contract doesn’t implement the required ERC721 interface, the transfer fails, preventing potential token loss. This function aims to prevent accidental transfers to contracts that are not compatible with ERC721 tokens.

**27-** How can an ERC1155 token be made into a non-fungible token?

- By limiting the supply of token ID to only one. In ERC1155, tokens are identified by their ID and amount. By restricting the amount of a specific token ID to 1, you effectively make it a non-fungible token as there's only one instance of it.

**28-** What is access control and why is it important?

- Access control ensures that only authorized users can execute specific functions or access sensitive parts of the contract. It prevents unauthorized users from manipulating critical parts of the contract's logic, thereby enhancing security and reducing the risk of attacks or exploits.

**29-** What does a modifier do?

- Modifiers are special functions that modify the behavior of other functions. They allow developers to add extra conditions or functionality without having to rewrite the entire function.

**30-** What is the largest value a uint256 can store?

- $2^{256} - 1$

**31-** What is variable and fixed interest rate?

- Fixed-rate financing means the interest rate on your loan does not change over the life of your loan. Variable-rate financing is where the interest rate on your loan can change, based on the prime rate or another rate called an “index".
