#	KYC - Know Your Customer
## A Decentralized KYC Verification Process for Banks.

### Smart Contract Flow
-	Bank collects the information for the KYC from Customer.

-	The information collected includes User Name and Customer data which is the hash link for the data present at a secure storage. This username and hash are unique for each customer. Though there could be multiple KYC requests of same username. 

-	A bank creates the request for submission which is stored in the smart contract.

-	A bank then verifies the customer KYC data which is then added to the customer list.

-	Other banks can get the customer information from the customer list.

-	Other banks can also provide votes on customer data, to showcase the authenticity of the data. These votes are then used to calculate customer rating and once this rating goes above 0.5 then the customer gets added to the final customer list which means that the customer is a trusted customer and such trusted customers are given additional benefits or offers by the bank.

-	Banks can also provide votes and ratings on other banks to showcase the authenticity of the banks. These ratings are important as KYC requests which are from banks with rating above 0.5 are only considered for validation. And banks with very poor rating might be removed by the admin.

### The folder structure of the project is as follows - ###

-	`Phase - 1` - contains the smart contract file `KYC.sol`
-	`Phase - 2` - contains 2 folders 
					1. kyc-ethereum-network 2. kyc-truffle-project
-	`kyc-ethereum-network` - contains the genesis block creation details for the network.
-	`kyc-truffle-project` - contains the migration scripts, smart contracts and network configuration file(`truffle-config.js`).

The smart contracts are written in ***Solidity programming language.*** The development and deployment of the KYC solidity smart contracts will follow the below stages - 

1.	Setting Up a Private Ethereum Blockchain.
2.	Deploying Smart Contracts on a Private Blockchain using Truffle.
3.	Testing the Smart Contracts on Private Blockchain.

### 1.	Setting Up a Private Ethereum Blockchain. ###
An Ethereum network is a blockchain built of multiple nodes. These nodes can exist anywhere in the world. Together, they constitute an Ethereum network.There are multiple such Ethereum networks; they are independent of each other, and anyone can create their own private Ethereum network. They are defined or identified by a unique parameter called Chain ID.

To connect to a network, you need to be aware of the chain ID of that network because you can only refer to the network using the network's unique chain ID. After this, you need to become a node for that network. For this, you need to download an Ethereum client on your machine and then connect to the network through that. This is important because you can only connect to a network as an Ethereum client. 

***A.	The `genesis.json` file present in the `kyc-ethereum-network` contains all the information needed to create a genesis block for the blockchain network. Run the below command inside `kyc-ethereum-network`. This command will create a private chain data for blockchain.***

>	geth --datadir ./datadir init ./genesis.json

The above command will generate the datadir folder.

***B.	Start geth client console using below command -***

>	geth --datadir ./datadir/ --networkid 2002 --rpc --rpcport 8545 --allow-insecure-unlock console

2002 is our network id and port is 8545.

***C.	Create eth accounts using below api’s , I have created 2 eth accounts with passwords set as below -***

>	personal.newAccount("admin")

>	personal.newAccount("HDFC")

>	personal.newAccount("KOTAK")

When the mining starts we need to unlock the accounts.

***D.	Start the mining process using the below command and keep the mining process running as we have to deploy KYC.sol on to this network.***

>	miner.start()


### 2.	Deploying Smart Contracts on a Private Blockchain using Truffle ###

Truffle's local development environment. Using Truffle, you will compile, deploy and test the KYC smart contracts on the Geth client. Once you have deployed a smart contract on a Geth client, the contract is available to the entire network.

`contracts` folder inside `kyc-truffle-project` contains the copy of the KYC solidity file. `migrations` folder contains the migration script for KYC smart contract. `truffle-config.js` file contains the network property settings.

A.	Compile the contract using the below command

>	truffle compile


B.	After the successful compilation, migrate the contract to geth

>	truffle migrate


C.	After successfully migrating contract, open truffle console using below command

>	truffle console


### 3.	Testing the Smart Contracts on Private Blockchain. ###

A.	When truffle console opens, get the instance of contract using below command

>	let kyc = await kyc.deployed()

`kyc` instance will have all the methods that was defined in sol file. We can call each function that we have defined in the KYC.sol file to carry the KYC operations.

For each transaction that we trigger you will get a receipt from the blockchain network. Below are the sequence of function execution for KYC process.

***1. Below are the 3 accounts that I have created.***

admin-0x37398bf5c3e6d42d4919b2517442bf025670ef82

HDFC-0xd71e9de2a1a47a4ff58558753551e87e55515d31

KOTAK-0x937f6af0edeaa4f912647e9a57568a2a97559fba


***2. Unlock admin account and execute the following to create the banks***

>	kyc.addBank(“HDFC”,”0xd71e9de2a1a47a4ff58558753551e87e55515d31”,”001”)

>	kyc.addBank(“KOTAK”,”0x937f6af0edeaa4f912647e9a57568a2a97559fba”,”002”)

***3. Unlock bank accounts and execute the following to upvote a bank-Upvote for DBS from HDFC, IDFC, KOTAK***

>	kyc.upvotesForBank(“0x2afcb67323d00536e6a8891deb667f38a34414f1”)

Rating of a bank goes up after upvotes from the other banks.

***4. Unlock HDFC bank account and execute the below command to add kyc request of a customer***

>	kyc.addRequest(“deepti”,”deeptikychash”)

Since the rating of bank DBS is higher DBS can add this customer.

***5. Unlock Kotak bank account and execute the below command to add kyc request of a customer***

>	kyc.addCustomer(“deepti”,”deeptikychash”)

At this point other banks can vote for this customer to verify the validity of the KYC document that this customer has submitted.

***6. Upvote customer to validate the KYC information from different banks-Upvote from HDFC, IDFC, KOTAK***

Unlock each account - HDFC, KOTAK and give a vote to the customer KYC information.

>	kyc.updateRatingCustomer(“deepti”)

Once the customer has enough votes he/she will be added to the valid KYC list.

***7. Verify valid KYC customers***

>	kyc.showFinalCustomer()

***8. Get ratings of a bank***

>	kyc.getBankRating(“0x2afcb67323d00536e6a8891deb667f38a34414f1”)

***9. Admin can remove a bank***

>	kyc.removeBank(“0xd71e9de2a1a47a4ff58558753551e87e55515d31”)

***10. Get the customer rating***

>	kyc.getCustomerRating(“deepti”)

***11. Set password for a customer***
>	kyc.setPasswordForCustomerData(“deepti”,”password”)

***12. Update customer KYC information***
>	kyc.modifyCustomer(“deepti”,”password”,”newDeeptiKYCHash”)

***13. Retrieve customer KYC history***
>	kyc.retrieveAccessHistory(“deepti”)

***14. Remove a customer in case of invalid KYC***
>	kyc.removeCustomer(“deepti”)


