#### I have created a set - a contract, a transaction and a script to answer questions 1 - 5 for this quest. This contract is to add an entry to a giveaway. Each entry includes the discord name, twitter name, email address and how many entries in this giveaway.

##### Contract deployed to 0x01 -
```cadence
pub contract Giveaway {

    pub var entries: {Address: Contestant}

    pub struct Contestant {
        pub var account: Address
        pub var discordName: String
        pub var twitterName: String
        pub var emailAddr: String
        pub var numberOfEntries: Int
    

        init(_account: Address, _discordName: String, _twitterName: String,
            _emailAddr: String, _numberOfEntries: Int) {
            self.account = _account
            self.discordName = _discordName
            self.twitterName = _twitterName
            self.emailAddr = _emailAddr
            self.numberOfEntries = _numberOfEntries
            }
    }
    pub fun newEntry(account: Address, discordName: String, twitterName: String, emailAddr: String, numberOfEntries: Int) {
        var giveawayEntry = Contestant(_account: account,_discordName: discordName, _twitterName: twitterName, _emailAddr: emailAddr, _numberOfEntries: numberOfEntries)
        self.entries[account] = giveawayEntry
    }

    init() {
        self.entries = {}
    }
}
```

##### Transaction to add an entry -
```cadence
import Giveaway from 0x01

transaction (account: Address, discordName: String, twitterName: String, emailAddr: String, numberOfEntries: Int)
{

  prepare(signer: AuthAccount) {}

  execute {
    Giveaway.newEntry(account: account, discordName: discordName, twitterName: twitterName, emailAddr: emailAddr, numberOfEntries: numberOfEntries)
    log(Giveaway.entries)
  }
}
```
##### Script to retrive information -
```cadence
import Giveaway from 0x01

pub fun main(account: Address): Giveaway.Contestant? {
  return Giveaway.entries[account]
}
```
