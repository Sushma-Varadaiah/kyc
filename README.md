### Smart Contract Flow
-	Bank collects the information for the KYC from Customer.

-	The information collected includes User Name and Customer data which is the hash link for the data present at a secure storage. This username and hash are unique for each customer. Though there could be multiple KYC requests of same username. 

-	A bank creates the request for submission which is stored in the smart contract.

-	A bank then verifies the customer KYC data which is then added to the customer list.

-	Other banks can get the customer information from the customer list.

-	Other banks can also provide votes on customer data, to showcase the authenticity of the data. These votes are then used to calculate customer rating and once this rating goes above 0.5 then the customer gets added to the final customer list which means that the customer is a trusted customer and such trusted customers are given additional benefits or offers by the bank.

-	Banks can also provide votes and ratings on other banks to showcase the authenticity of the banks. These ratings are important as KYC requests which are from banks with rating above 0.5 are only considered for validation. And banks with very poor rating might be removed by the admin.

