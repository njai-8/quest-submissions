#### Explain what lives inside of an account.
There are 2 areas in an account on Flow blockchain - The <b>contract area</b> and the <b>account storage</b>. The contract area is for the smart contracts and the contract interfaces. The account storage is for objects belongs to the account. It consist of 3 domains - /storage/, /private/ and /public/.

#### What is the difference between the /storage/, /public/, and /private/ paths?
/storage/ - it is for storing the object, like the NFT owned by the account owner. It can be access by the account owner only.

/public/ - anyone in the network can access this path. 

/private/ - only selective individual can access this path. links can be created her to stored assets so the assigned parties can have access.

#### What does .save() do? What does .load() do? What does .borrow() do?
.save() - The save function is for saving data to an account. The destination has to be in /storage/

.load() - The load function is for retrieving data from the account storage. 

.borrow() - The borrow function gets a reference to the resource instead of the actual resource. This way the resource will not be taken out of the storage.

#### Explain why we couldn't save something to our account storage inside of a script.
It is because we need to use the AuthAccount to save something into storage. In script, we can only use the Public Account, and thus we cannot save something to storage using a script. Also transactions are for creating and updating the data on the blockchain, while scripts are for viewing the information/data on the blockchain.

#### Explain why I couldn't save something to your account.
It is because only the account owner can sign the transaction to save data to an account.

#### Define a contract that returns a resource that has at least 1 field in it. Then, write 2 transactions: 
1. A transaction that first saves the resource to account storage, then loads it out of account storage, logs a field inside the resource, and destroys it.
2. A transaction that first saves the resource to account storage, then borrows a reference to it, and logs a field inside the resource.

##### Contract
```cadence
pub contract fruitNFT {

    pub resource fruit {
        pub let id: UInt64
        pub let fruitname: String
        pub let color: String

        init(_id: UInt64, _fruitname: String, _color: String) {
            self.id = _id
            self.fruitname = _fruitname
            self.color = _color
        }
    }

    pub fun newNFT(id: UInt64, fruitname: String, color: String): @fruit {
        return <- create fruit(_id : id, _fruitname : fruitname, _color: color)
    }
}
```
##### Save and Load
```cadence
import fruitNFT from 0x01

transaction(id: UInt64, fruitname: String, color: String) {

  prepare(signer: AuthAccount) {
    let fruit <- fruitNFT.newNFT(id: id, fruitname: fruitname, color: color)

    signer.save(<- fruit, to: /storage/fruitNFT)

    log("New fruit NFT saved")

    let myfruit <- signer.load<@fruitNFT.fruit>(from: /storage/fruitNFT) ?? panic ("No such NFT!")

    log(myfruit.id)
    log(myfruit.fruitname)
    log(myfruit.color)

    destroy myfruit

  }

  execute {
  }
}
```

##### Save and borrow
```cadence
import fruitNFT from 0x01

transaction(id: UInt64, fruitname: String, color: String) {

  prepare(signer: AuthAccount) {
    let fruit <- fruitNFT.newNFT(id: id, fruitname: fruitname, color: color)

    signer.save(<- fruit, to: /storage/fruitNFT)

    log("New fruit NFT saved")

    let myfruit = signer.borrow<&fruitNFT.fruit>(from: /storage/fruitNFT) ?? panic ("No such NFT!")

    log(myfruit.id)
    log(myfruit.fruitname)
    log(myfruit.color)
  }

  execute {
  }
}
```
