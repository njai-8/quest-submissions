#### Explain, in your own words, the 2 things resource interfaces can be used for (we went over both in today's content)

The 2 things that interfaces can be used for are -
1. It can be used to setup standards and requirements, so that can be enforced across different smart contracts, and;
2. It can be used to control which of the variables and functions to be exposed so we can choose who can use what in the smart contracts

#### Define your own contract. Make your own resource interface and a resource that implements the interface. Create 2 functions. In the 1st function, show an example of not restricting the type of the resource and accessing its content. In the 2nd function, show an example of restricting the type of the resource and NOT being able to access its content.

```cadence
pub contract catNFTs {

    pub resource interface IcatNFT {
        pub var id: UInt64
        pub var catName: String
    }

    pub resource catNFT: IcatNFT {
        pub var id: UInt64
        pub var catName: String
        pub var gender: String
        pub var color: String
        pub var age: UInt64

        init() {
            self.id = 1
            self.catName = "Garfield"
            self.gender = "M"
            self.color = "Ginger"
            self.age = 4
        }


    }

    pub fun getcatcolor() {
        //Interface is not being used so this function can access all the fields in the resource
        let cat: @catNFT <- create catNFT()
        let catcolor = cat.color
        log(catcolor)

        destroy cat

    }

    pub fun getcatage() {
        //Using Interface IcatNFT. This interface only expose two fields - id and catName. 
        //age is not exposed so this function has the error 
        //"cannot find variable in this scope: catage. not found in this scope"
        let cat: @catNFT{IcatNFT} <- create catNFT()
        let catage = cat.age
        log(catage)

        destroy cat
    }
}
```


#### How would we fix this code?
```cadence
pub contract Stuff {

    pub struct interface ITest {
      pub var greeting: String
      pub var favouriteFruit: String 
//added line - Need to expose the function 'changeGreeting' so fixThis() can use      
      pub fun changeGreeting(newGreeting: String): String
    }

    // ERROR:
    // `structure Stuff.Test does not conform 
    // to structure interface Stuff.ITest`
    pub struct Test: ITest {
      pub var greeting: String
//added line - The ITest exposed something that is not in Test
      pub var favouriteFruit: String

      pub fun changeGreeting(newGreeting: String): String {
        self.greeting = newGreeting
        return self.greeting // returns the new greeting
      }

      init() {
        self.greeting = "Hello!"
//added line - to init the variable
        self.favouriteFruit = "apple"
      }
    }

    pub fun fixThis() {
      let test: Test{ITest} = Test()
      let newGreeting = test.changeGreeting(newGreeting: "Bonjour!") // ERROR HERE: `member of restricted type is not accessible: changeGreeting`
      log(newGreeting)
    }
}
```
