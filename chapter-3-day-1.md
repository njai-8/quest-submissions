#### 1. In words, list 3 reasons why structs are different from resources.
Structs and Resources are both containers of data but they have the following difference -
1. Structs can be copied - we can copy the vaule in Structs and both the new and old Sturct will exist at the same time. For Resources, the value has to be moved and once it is moved the original Resource will not exist anymore.
2. When trying to move a Resource into another resource that is not empty, an error will be generated. This is to prevent any Resource to be lost in the transaction. Structs do not have this constraint.
3. Stucts can be created any where any time in the code. Generally it is the same as any other data type we can define it and use it whenever is needed. A Resources is a lot more controlled. We have to use the keyword 'create' to create inside the contract. 

#### 2. Describe a situation where a resource might be better to use than a struct.

#### 3. What is the keyword to make a new resource?
The keyword to make a new resource is 'create'

#### 4. Can a resource be created in a script or transaction (assuming there isn't a public function to create one)?
No, it has to be created inside a contract, so the developer can control when they are created.

#### 5. What is the type of the resource below?
Is it a trick question? I am not sure I get this question but Resource should not be of any type (or can be any type) as it is a container of data. I believe it's the data it contains that has a data type? 

#### 6.
```cadence
pub contract Test {

    // Hint: There's nothing wrong here ;)
    pub resource Jacob {
        pub let rocks: Bool
        init() {
            self.rocks = true
        }
    }

    //pub fun createJacob(): Jacob { // there is 1 here
    pub fun createJacob(): @Jacob {
        //let myJacob = Jacob() // there are 2 here
        let myJacob <- create Jacob()
        //return myJacob // there is 1 here
        return <- myJacob
    }
}
```