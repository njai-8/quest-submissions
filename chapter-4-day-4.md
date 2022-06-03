#### Because we had a LOT to talk about during this Chapter, I want you to do the following:

Take our NFT contract so far and add comments to every single resource or function explaining what it's doing in your own words. Something like this:

```cadence
pub contract CryptoPoops {
  pub var totalSupply: UInt64

  // This is an NFT resource that contains a name,
  // favouriteFood, and luckyNumber
  // the id of the NFT is uuid
  pub resource NFT {
    pub let id: UInt64

    pub let name: String
    pub let favouriteFood: String
    pub let luckyNumber: Int

    init(_name: String, _favouriteFood: String, _luckyNumber: Int) {
      self.id = self.uuid

      self.name = _name
      self.favouriteFood = _favouriteFood
      self.luckyNumber = _luckyNumber
    }
  }

  // This is a resource interface that allow 
  pub resource interface CollectionPublic {
    pub fun deposit(token: @NFT)
    pub fun getIDs(): [UInt64]
    pub fun borrowNFT(id: UInt64): &NFT
  }
  // This is to define a collection of the NFTs. It is like a folder so this
  // can hold multiple NFTs of the same kind.
  pub resource Collection: CollectionPublic {
    pub var ownedNFTs: @{UInt64: NFT}

    // This is to deposit a token into the dictionary / Collection
    pub fun deposit(token: @NFT) {
      self.ownedNFTs[token.id] <-! token
    }
    // This is to withdraw a token out of the collection
    // using the id as key
    // it move the token out and return it
    // The ?? panic is required for nothing to withdraw from 
    // collection
    pub fun withdraw(withdrawID: UInt64): @NFT {
      let nft <- self.ownedNFTs.remove(key: withdrawID) 
              ?? panic("This NFT does not exist in this Collection.")
      return <- nft
    }
    // This funtion return an array which includes all the IDs of NFT in the Collection
    pub fun getIDs(): [UInt64] {
      return self.ownedNFTs.keys
    }
    // This function returns a reference to the NFT
    // inside the collection. This way users can read the information
    // of NFT without having to withdrawing it from the collection.
    // This also can be used in script
    pub fun borrowNFT(id: UInt64): &NFT {
      return &self.ownedNFTs[id] as &NFT
    }

    init() {
      self.ownedNFTs <- {}
    }

    destroy() {
      destroy self.ownedNFTs
    }
  }
  // This is to create a new Collection for storing NFTs
  pub fun createEmptyCollection(): @Collection {
    return <- create Collection()
  }
  // This resource is to wrapped around the createNFT to make sure this function can only 
  // be called by the account owner. So no one other than the account owner can mint the NFT.
  // It is being done by creating this resource in the /storage/Minter which only account owner can access
  pub resource Minter {
    // This function is to create an NFT
    pub fun createNFT(name: String, favouriteFood: String, luckyNumber: Int): @NFT {
      return <- create NFT(_name: name, _favouriteFood: favouriteFood, _luckyNumber: luckyNumber)
    }
    // This function is to create a Minter resource
    pub fun createMinter(): @Minter {
      return <- create Minter()
    }

  }

  init() {
    self.totalSupply = 0
    self.account.save(<- create Minter(), to: /storage/Minter)
  }
}
```
