#### Why did we add a Collection to this contract? List the two main reasons.
The Collection resource is being used to act as a folder to store all differnt NFTs. Because if the NFTs are being stored individually we need to store them in separate /storage/XXXXX path, as each storage can only store one NFT, and this is very inefficient. By using a collection resource we can eliminate this problem.
Also, we can allow other users to deposit into the collection.


#### What do you have to do if you have resources "nested" inside of another resource? ("Nested resources")
A destroy() function has to be added to the outermost resource to destroy the resource inside of it. This is to ensure when this outermost resource is destroyed, the ones inside it will also be destroyed.

#### Brainstorm some extra things we may want to add to this contract. Think about what might be problematic with this contract and how we could fix it.
* Idea #1: Do we really want everyone to be able to mint an NFT? 🤔.
* Idea #2: If we want to read information about our NFTs inside our Collection, right now we have to take it out of the Collection to do so. Is this good?

With this contract, the only way to get the information of an NFT is to use the withdraw function. This means we need to move the resource out of the storage everytime even when the only thing we need is just to read some of the metadata, and have to deposit it back to the collection. This is not efficient. To fix this we can add a function to return the reference to an NFT inside the collection, expose it to the public, so users can use the NFT id to get the information inside the NFT.

Also this contract allow everyone to sign the transaction and mint the NFT and deposit it to the user's own account. This is not good as we cannot control who can mint the NFT. To fix this is can put the function that creates the NFT in a new resource, and save this resource in the /storage/ area. This way, only the account owner will have acceses to this function to mint an NFT.
