*1. What does .link() do?*

    Function .link() allow access to data in /storage/ throw /public/ or /private/ hardlinks like in Linux according to functional restrictions  

*2. In your own words (no code), explain how we can use resource interfaces to only expose certain things to the /public/ path.*
    
    We can declare access to certain parts of resource to read or modify. 

*3. Deploy a contract that contains a resource that implements a resource interface. Then, do the following:*

    pub contract Quest {

        pub resource interface ILinks {
            pub var zwei: Int
            pub var drei:Int
            pub fun changeLink(newLink1: Int, newLink2: Int) 
        }

        pub resource Links: ILinks {

            pub var zwei: Int
            pub var drei: Int
            pub var vier: Int

            pub fun changeLink(newLink1: Int, newLink2: Int) {
                self.drei = newLink1
                self.vier = newLink2
            }

            init() {
                self.zwei = 2
                self.drei = 3
                self.vier = 4
            }
        }

        pub fun createLinks(): @Links {
            return <- create Links()
        }
    }


*In a transaction, save the resource to storage and link it to the public with the restrictive interface.*

    import Quest from 0x01

    transaction(address: Address, Link1: Int, Link2: Int) {

      prepare(signer: AuthAccount) {

       let testLinks <- Quest.createLinks()
       signer.save(<- testLinks, to: /storage/TestLinks)
       signer.link<&Quest.Links{Quest.ILinks}>(/public/TestLinks, target: /storage/TestLinks)

      }

      execute {
        let testResource: &Quest.Links{Quest.ILinks} = 
          getAccount(address).getCapability<&Quest.Links{Quest.ILinks}>(/public/TestLinks).borrow() ?? panic("Error")

        testResource.changeLink(newLink1: Link1, newLink2: Link2)

        log(testResource.drei)

      }
    }

*Run a script that tries to access a non-exposed field in the resource interface, and see the error pop up.*

    import Quest from 0x01
    pub fun main(address: Address): Int {

      let testResource: &Quest.Links{Quest.ILinks} =
      getAccount(address).getCapability<&Quest.Links{Quest.ILinks}>(/public/TestLinks).borrow() ?? panic("Error")

      return testResource.vier //ERROR MEMBER OF RESTRICTED TYPE IS NOT ACCESSIBLE
    }

*Run the script and access something you CAN read from. Return it from the script.*

    import Quest from 0x01
    pub fun main(address: Address): Int {

      let testResource: &Quest.Links{Quest.ILinks} =
      getAccount(address).getCapability<&Quest.Links{Quest.ILinks}>(/public/TestLinks).borrow() ?? panic("Error")

      return testResource.zwei
    }
