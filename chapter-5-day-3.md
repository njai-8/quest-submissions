### What does "force casting" with as! do? Why is it useful in our Collection?
Using the 'force casting' operator `as!` means downcasting a generic type to a more specific type. In Collections interface, the type is usually generic and we cannot access the data from there. by downcasting it to a more specific type we will be able to get to the metadata. 

### What does auth do? When do we use it?
auth states that the reference after it is a authorized reference. If we want to downcast a generic type to a more specific type we need to use the authorized reference.

### This last quest will be your most difficult yet. Take this contract:

#### Contract CryptoPoops
```cadence
import NonFungibleToken from 0x02
pub contract CryptoPoops: NonFungibleToken {
  pub var totalSupply: UInt64

  pub event ContractInitialized()
  pub event Withdraw(id: UInt64, from: Address?)
  pub event Deposit(id: UInt64, to: Address?)

  pub resource NFT: NonFungibleToken.INFT {
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
  //NEW
  pub resource interface ICollection {
    pub fun borrowAuthNFT(id: UInt64): &NFT 
  }

  pub resource Collection: NonFungibleToken.Provider, NonFungibleToken.Receiver, NonFungibleToken.CollectionPublic, ICollection {
    pub var ownedNFTs: @{UInt64: NonFungibleToken.NFT}

    pub fun withdraw(withdrawID: UInt64): @NonFungibleToken.NFT {
      let nft <- self.ownedNFTs.remove(key: withdrawID) 
            ?? panic("This NFT does not exist in this Collection.")
      emit Withdraw(id: nft.id, from: self.owner?.address)
      return <- nft
    }

    pub fun deposit(token: @NonFungibleToken.NFT) {
      let nft <- token as! @NFT
      emit Deposit(id: nft.id, to: self.owner?.address)
      self.ownedNFTs[nft.id] <-! nft
    }

    pub fun getIDs(): [UInt64] {
      return self.ownedNFTs.keys
    }

    pub fun borrowNFT(id: UInt64): &NonFungibleToken.NFT {
      return &self.ownedNFTs[id] as &NonFungibleToken.NFT
    }

    pub fun borrowAuthNFT(id: UInt64): &NFT {
      let ref = &self.ownedNFTs[id] as auth &NonFungibleToken.NFT
      return ref as! &NFT
    }

    init() {
      self.ownedNFTs <- {}
    }

    destroy() {
      destroy self.ownedNFTs
    }
  }

  pub fun createEmptyCollection(): @NonFungibleToken.Collection {
    return <- create Collection()
  }

  pub resource Minter {

    pub fun createNFT(name: String, favouriteFood: String, luckyNumber: Int): @NFT {
      return <- create NFT(_name: name, _favouriteFood: favouriteFood, _luckyNumber: luckyNumber)
    }

    pub fun createMinter(): @Minter {
      return <- create Minter()
    }

  }

  init() {
    self.totalSupply = 0
    emit ContractInitialized()
    self.account.save(<- create Minter(), to: /storage/Minter)
  }
}
```

#### Transaction to Create Collection
```cadence
import CryptoPoops from 0x01
import NonFungibleToken from 0x02

transaction() {

  prepare(acct: AuthAccount) {
    acct.save(<- CryptoPoops.createEmptyCollection(), to: /storage/PoopsCollection)
    acct.link<&CryptoPoops.Collection{NonFungibleToken.Receiver, NonFungibleToken.CollectionPublic, CryptoPoops.ICollection}>(/public/PoopsCollection, target: /storage/PoopsCollection)
  }
  execute {
    log("Collection created")
  }
}
```

#### Transaction to Mint NFT
```cadence
import CryptoPoops from 0x01
import NonFungibleToken from 0x02

transaction(name: String, favouriteFood: String, luckyNumber: Int, recipient: Address) {
    prepare(admin: AuthAccount) {
        let adminRef = admin.borrow<&CryptoPoops.Minter>(from: /storage/Minter)
                    ?? panic("Not Admin!")
        let nft <- adminRef.createNFT(name: name, favouriteFood: favouriteFood, luckyNumber: luckyNumber)

        let myCollectionRef = getAccount(recipient).getCapability(/public/PoopsCollection)
                                .borrow<&CryptoPoops.Collection{NonFungibleToken.Receiver}>()
                                ?? panic("No such collection")
        myCollectionRef.deposit(token: <- nft)
    }

    execute{
        log("minted")
    }
}

```

#### Script to Get ID
```cadence
import CryptoPoops from 0x01
import NonFungibleToken from 0x02

pub fun main(address: Address): [UInt64] {
  let publicCollection = getAccount(address).getCapability(/public/PoopsCollection)
                          .borrow<&CryptoPoops.Collection{NonFungibleToken.CollectionPublic}>()
                            ?? panic("This cannot be access over a link")
  return publicCollection.getIDs()


}
```

#### Script to Get Metadata
```cadence
import CryptoPoops from 0x01
import NonFungibleToken from 0x02

pub fun main(address: Address, id:UInt64): &CryptoPoops.NFT {
    let publicCollection = getAccount(address).getCapability(/public/PoopsCollection)
                            .borrow<&CryptoPoops.Collection{CryptoPoops.ICollection}>()
                            ?? panic("This cannot be access over a link")
    let nftRef: &CryptoPoops.NFT = publicCollection.borrowAuthNFT(id: id)
    return nftRef
}                                
```
