#### Contract
```cadence
pub contract ManageBooks {

//DICTIONARY

    pub resource Library {
        pub var Books: @{String: BookRecord}

        pub fun checkinbook(bookrecord: @BookRecord) {
            self.Books[bookrecord.BookName] <-! bookrecord
        }

        pub fun checkoutbook(key: String): @BookRecord{
            let bookrecord <- self.Books.remove(key: key) ?? panic("no such book")
            return <- bookrecord
        }

        init() {
            self.Books <- {}
        }

        destroy() {
            destroy self.Books
        }
    }

    pub resource BookRecord {
        pub var BookName: String
        pub var NumOfPages: Int
        pub var Author: String
        init(_bookname: String, _numofpages: Int, _author:String) {
            self.BookName = _bookname
            self.NumOfPages = _numofpages
            self.Author = _author
        }
    }

    pub fun createLibrary(): @Library {
        var testLibrary: @Library <- create Library()

        return <- testLibrary
    }


    pub fun addBook(bookname: String, numofpages: Int, author: String): @BookRecord {
        var testResource: @BookRecord <- create BookRecord(_bookname: bookname, _numofpages: numofpages, _author: author)

        return <- testResource
    }

// ARRAY
   
   pub var BookClub: @[Members]

    pub resource Members {
        pub let MemberName: String

        init(MemberName: String) {
            self.MemberName = "first member"
        }
    }

    pub fun addMember(MemberName: String): @Members {
        return <- create Members(MemberName:MemberName)
    }

    pub fun memberjoin(members: @Members) {
        self.BookClub.append(<- members)
    }
    
    pub fun memberleave(index: Int): @Members {
        return <- self.BookClub.remove(at: index)
    }

    init() {
        self.BookClub <- []
    }


}
```
#### Transaction to Add to array ->
```cadence
import ManageBooks from 0x01

transaction (MemberName: String) {

  prepare(acct: AuthAccount) {}

  execute {
    let newMember <- ManageBooks.addMember(MemberName: MemberName)

    ManageBooks.memberjoin(members: <- newMember)

    log("Member join club")


    
  }
}
```
