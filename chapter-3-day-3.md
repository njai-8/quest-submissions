```cadence
pub contract catNFTs {

    pub var NFTset: @{UInt64: catNFT}

    pub fun catReference(key: UInt64): &catNFT {
        return &self.NFTset[key] as &catNFT
    }

    pub fun addNFT(id: UInt64, catName: String, gender: String, color: String, age: UInt64) {
        let catNFTdetail <- create catNFT(_id: id, _catName: catName, _gender: gender, _color: color, _age: age)
        self.NFTset[catNFTdetail.id] <-! catNFTdetail
    }

    pub resource catNFT {
        pub var id: UInt64
        pub var catName: String
        pub var gender: String
        pub var color: String
        pub var age: UInt64

        init(_id: UInt64, _catName: String, _gender: String, _color: String, _age: UInt64) {
            self.id = _id
            self.catName = _catName
            self.gender = _gender
            self.color = _color
            self.age = _age
        }
    }

    init(){
        self.NFTset <- {}
    }
}
```
#### script that reads information
```cadence
import catNFTs from 0x01

pub fun main(key: UInt64): &catNFTs.catNFT {
  
  return catNFTs.catReference(key: key)
}
```
#### Explain, in your own words, why references can be useful in Cadence.
References can be useful in Cadence especially when dealing with resources. There are a lot of times that we need to retrieve information from a resource, but do not actually want to touch it by moving around or risk losing it. For example, when we want to retrieve the metadata of an NFT, we can use reference so that we do not have deal with the actual resource and risk unnecessary change to the NFT or even lose it.
