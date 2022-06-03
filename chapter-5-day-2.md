#### Explain why standards can be beneficial to the Flow ecosystem.
Standards can be beneficial to the Flow ecosystem in many ways. First of all using the NFT contract standard ensure everyone is using the same way to interact with contracts, and that saves a lot of time troubleshooting when issue occurs. Also when there is an update to the blockchain, the update to the contracts and transactions will be less painful, as we are using the standard set of functions and resources. It will be easier for developer to put together good quality codes, and whoever has to support it afterwards will have an easier life. 

#### What is YOUR favourite food?
Are you buying me lunch? I'd like to have a fish burrito with extra Guacamole :)

#### Please fix this code (Hint: There are two things wrong):
The two problems are -
In the interface :
```
  pub resource Stuff {
    pub var favouriteActivity: String
  }
```
should be
```
  pub resource Stuff: IStuff {
    pub var favouriteActivity: String
  }
```
In the implementing contract:
```cadence
// The contract should implement ITest
pub contract Test: ITest {
  pub var number: Int
  
  pub fun updateNumber(newNumber: Int) {
    self.number = 5
  }

  pub resource interface IStuff {
    pub var favouriteActivity: String
  }

  pub resource Stuff: IStuff {
    pub var favouriteActivity: String

    init() {
      self.favouriteActivity = "Playing League of Legends."
    }
  }

  init() {
    self.number = 0
  }
}
```
