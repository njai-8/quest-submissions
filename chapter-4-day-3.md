#### Why did we add a Collection to this contract? List the two main reasons.
The Collection resource is being used to act as a folder to store all differnt NFTs. Because if the NFTs are being stored individually we need to store them in separate /storage/XXXXX path, as each storage can only store one NFT, and this is very inefficient. By using a collection resource we can eliminate this problem.
Also, we can allow other users to deposit into the collection.


#### What do you have to do if you have resources "nested" inside of another resource? ("Nested resources")
A destroy() function has to be added to the outermost resource to destroy the resource inside of it. This is to ensure when this outermost resource is destroyed, the ones inside it will also be destroyed.

#### Brainstorm some extra things we may want to add to this contract. Think about what might be problematic with this contract and how we could fix it.
* Idea #1: Do we really want everyone to be able to mint an NFT? 🤔.
* Idea #2: If we want to read information about our NFTs inside our Collection, right now we have to take it out of the Collection to do so. Is this good?

With this contract, everyone can use a transaction to call the withdraw function and withdraw an NFT from the collection, because the resource type it uses expose everything in it. This is not a good idea. If we use resource interface, we can define what variables and functions to be expose, and that way other people cannot access the fucntions that they are not suppose to and mess with the collection.

Also in this contract, the only way to get the information of an NFT is to use the withdraw function.
