*1. In words, list 3 reasons why structs are different from resources.*

    Resources are unchangable, non-copied and set up in contract, so they are unique structures, that can only be use(moved or destroyed) once.
    
*2. Describe a situation where a resource might be better to use than a struct.*

    ID- or other authority cards, archive lists, NFT's, rules

*3. What is the keyword to make a new resource?*

    create

*4. Can a resource be created in a script or transaction (assuming there isn't a public function to create one)?*

    no

*5. What is the type of the resource below?*

    @Jacob
    
*6. Let's play the "I Spy" game from when we were kids. I Spy 4 things wrong with this code. Please fix them.*

    pub contract Test {

    // Hint: There's nothing wrong here ;)
    pub resource Jacob {
        pub let rocks: Bool
        init() {
            self.rocks = true
        }
    }

    pub fun createJacob(): @Jacob { // there is 1 here: no @
        let myJacob <- create Jacob() // there are 2 here: = and no create
        return <- myJacob // there is 1 here: no <-
    }
}


    

