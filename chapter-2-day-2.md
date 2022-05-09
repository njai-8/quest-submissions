#### 1. Explain why we wouldn't call changeGreeting in a script.
In blockchain application, transactions are for creating and updating the data on the blockchain, while scripts are for viewing the information/data on the blockchain. The objective of the function 'changeGreeting' is to modify the data stored in the variable greeting, and thus, is changing the data on the blockchain, therefore it should be called in a transaction, not script.

#### 2. What does the AuthAccount mean in the prepare phase of the transaction?
AuthAccount in the prepare phase refers to the account that requested this transaction. For example, we want to access the data for this certain account.

#### 3. What is the difference between the prepare phase and the execute phase in the transaction?
Prepare phase is like we logon to an account in web2 or local application, we identify who (which account) the request is for and we can access the data/information for this account. Execute phase is where the actual transaction happen, it add/change/update data on the blockchain on behalf of the account identified in the Prepare phase. And while in actual code, thing that are supposed to be done in the execute phase can be done in the prepare phase as well, but using the execute phase can make cleaner codes and easier to read.

### 4.
Contract -
```cadence
pub contract HelloWorld {
    pub var greeting: String
    pub var myNumber: Int

    pub fun changeGreeting(newGreeting: String) {
        self.greeting = newGreeting
    }

    pub fun updateMyNumber(newNumber: Int) {
        self.myNumber = newNumber
    }
    
    init() {
        self.greeting = "Hello, World!"
        self.myNumber = 0 //init value is 0
    }
}
```

Script - 
```cadence
import HelloWorld from 0x01

pub fun main(): Int {
  return HelloWorld.myNumber
}
```

Transaction -
```cadence
import HelloWorld from 0x01

transaction (myNewNumber: Int) {

  prepare(signer: AuthAccount) {}

  execute {
    HelloWorld.updateMyNumber(newNumber: myNewNumber)
    log(HelloWorld.myNumber)
  }
}
```

Result -
```
17:00:24 New Transaction 999
```
