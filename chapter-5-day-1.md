#### Describe what an event is, and why it might be useful to a client.
Event is like a broadcast from a contract, so it can proactivly sends out information for a client who wants to use it. An example is the Float live page, we see a float icon floating on the page when someone mints a float. That information is from an event.

#### Deploy a contract with an event in it, and emit the event somewhere else in the contract indicating that it happened.
#### Using the contract in step 2), add some pre conditions and post conditions to your contract to get used to writing them out.
I'd like to answer the 2 questions in one combined answer -
##### contract
```cadence
pub contract Room {

    pub event Visit(name: String)


    pub fun visits(visitor: String){
        pre {
            visitor != "Jack": "Jack cannot come in"
        }
        post {
            greetings == "Jacob, Welcome!": "Only Jacob can come in"
        }
        log(visitor)
        log("Welcome")
        var greetings: String = visitor.concat(", Welcome!")
        log(greetings)
        //emit Visit(name: visitor)
    }
    init() {

        }
    

}
```

##### script
```cadence
import Room from 0x01

pub fun main(people:String) {
  
  Room.visits(visitor:people)
  
}
```

##### results

![image](https://user-images.githubusercontent.com/104394884/171930249-238b9659-84f0-4143-9cec-97265eb2084c.png)


#### For each of the functions below (numberOne, numberTwo, numberThree), follow the instructions.

##### numberOne will log the name. 
It is because the precondition is the input name has to be 5 characters in length. Since "Jacob" has 5 letters it satisified the precondition. 
Anything that is 5 characters long e.g. "12345", "abcde", "xxxxx" could pass this precondition.

##### numberTwo will return a value "Jacob Tucker".
The precondition check the input name cannot be null (length > or = 0). Most input could pass this precondition, including "Jacob".
The postcondition checks for the result = "Jacob Tucker". Now we need to check what the function does. It concatenate the name with " Tucker". If the input name is "Jacob", the result will be "Jacob Tucker", and thus the postcondition is met and the function will return the value "Jacob Tucker".

##### numberThree will not log the updated number.
After the function is run, the self.number is 0 because the postcondition is not met. Because - <br/>
At start, ```self.number = 0```(from init()).<br/>
thus, ```before(self.number) = 0``` (before the function is being run)<br/>
the function does this one thing ```self.number = self.number + 1```, and then ```return self.number```. So the result value should be <b>0 + 1, which is 1.</b><br/>
Now in the postcondition, it checks for ```before(self.number) (which is 0) == result (which is 1) + 1```. <b>Since 0 != 2, the poscondition cannot pass</b>. <br/>
In such case, everything is reverted and the <b>self.number reverts back to 0</b>.<br/>
if we change the postcondition to before(self.number) == result - 1 the result will be returned.<br/>
