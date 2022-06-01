#### What does .link() do?
Link() creates a capability to a public or private path from a storage path. In other words, it takes the data in the storage path, and expose a reference to it which can be access from the public or privat path.

#### In your own words (no code), explain how we can use resource interfaces to only expose certain things to the /public/ path.
Using resource interface can allow us to expose only certain information in the resource to the public. For example, there are 2 fields, ID and name in a resource, we can use interface to expose only the ID to the public. Combining the resource interface with the link function, we can take something in the storage, reference it in the public path for the public to use, but only expose some data but defining the resource interface.

#### Deploy a contract that contains a resource that implements a resource interface. Then, do the following:
* In a transaction, save the resource to storage and link it to the public with the restrictive interface.
* Run a script that tries to access a non-exposed field in the resource interface, and see the error pop up.
* Run the script and access something you CAN read from. Return it from the script.

##### Contract
```cadence
pub contract fruitNFT {

    pub resource interface Ifruit {
        pub let id: UInt64
        pub let fruitname: String
    }

    pub resource fruit: Ifruit {
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
##### Transaction to save and link
```cadence
import fruitNFT from 0x01

transaction(id: UInt64, fruitname: String, color: String) {

  prepare(signer: AuthAccount) {
    let fruit <- fruitNFT.newNFT(id: id, fruitname: fruitname, color: color)

    signer.save(<- fruit, to: /storage/fruitNFT)
    log("New fruit NFT saved")

    signer.link<&fruitNFT.fruit{fruitNFT.Ifruit}>(/public/publicfruit, target: /storage/fruitNFT)
    log("fruit linked to public")

  }

  execute {
  }
}
```
##### Data Entered 
* id : 0
* fruitname : banana
* color : yellow

##### script to read unexposed field (color)
```cadence
import fruitNFT from 0x01

pub fun main(): String {
  let publicfruit = getAccount(0x02).getCapability(/public/publicfruit).borrow<&fruitNFT.fruit{fruitNFT.Ifruit}>()
                      ?? panic("No public fruit available for you!")
  return publicfruit.color
}
```
It errored at the 'return' line, saying 'member of restricted type is not accessible: color', because it is not exposed from the resource interface.
When the line change to return publicfruit.fruitname -
```cadence
import fruitNFT from 0x01

pub fun main(): String {
  let publicfruit = getAccount(0x02).getCapability(/public/publicfruit).borrow<&fruitNFT.fruit{fruitNFT.Ifruit}>()
                      ?? panic("No public fruit available for you!")
  return publicfruit.fruitname
}
```
It works and produce the following output 
```
24:58:55 
Script 
Result
{"type":"String","value":"banana"}
```
