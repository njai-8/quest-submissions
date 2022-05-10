#### 1.In a script, initialize an array (that has length == 3) of your favourite people, represented as Strings, and log it.
```cadence
pub fun main(){
    var s: [String] = ["Iron Man","Thor","Hulk"]
    log(s)
}
```
##### Result
```
22:25:29 New Script ["Iron Man", "Thor", "Hulk"]
22:25:29 New Script Result {"type":"Void"}
```

#### 2.In a script, initialize a dictionary that maps the Strings Facebook, Instagram, Twitter, YouTube, Reddit, and LinkedIn to a UInt64 that represents the order in which you use them from most to least. For example, YouTube --> 1, Reddit --> 2, etc. If you've never used one before, map it to 0!
```cadence
pub fun main(): UInt64 {
    var s : {String:UInt64} = {"Facebook":1, "Instagram":2, "YouTube":3, "Reddit":5, "Linkedln":0}
    log(s)
    return s["Reddit"]!
}
```

##### Result
```
22:27:41 New Script {"Instagram": 2, "Linkedln": 0, "Facebook": 1, "YouTube": 3, "Reddit": 5}
22:27:41 New Script Result {"type":"UInt64","value":"5"}
```

#### 3.Explain what the force unwrap operator ! does, with an example different from the one I showed you (you can just change the type).
The force unwrap operator (!) unwraps an optional variable so the value can be used. From the script above in #2, The UInt64 value in the dictionary s is an optional value as it can be nil. In order to use the value that is being stored we need to unwrap it with the ! operator. 

#### 4.Using this picture below, explain...
  * What the error message means
  * Why we're getting this error
  * How to fix it

The error message saying "mismatched types. expected String, got String?" mean that the value that is being assigned to the return value has a different type. In the function, we defined the return value to have a data type of "String". However the value that is being assigned is from the dictionary "thing" and is an optional String (String?) as it can be nil. There are 2 ways to fix this error -
1. to unwrap the value we get from the dictionary, that means change line 3 to 
``` cadence
return thing[0x03]!
```

-OR-

2. Change the data type of the return value to an wrapped String, that means change line 1 to
```cadence
pub fun main(): String? {
```
