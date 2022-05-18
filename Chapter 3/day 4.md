*1. Explain, in your own words, the 2 things resource interfaces can be used for (we went over both in today's content)*

Interfaces are used for unvailing only what you have decided to unvail from resourse in certain conditions and you can do it in variations using them in types.

*2. Define your own contract.*

    pub contract Quest {

        pub resource Scores: IScores {
            pub var score: Int
            pub var name: String
            pub var future: Bool

            pub fun changeScore(_score: Int): Int {
                self.score = _score
                return self.score
            }

            pub fun changeFuture(_future: Bool): Bool {
                self.future = _future
                return self.future
            }

            init() {
                self.score = 6
                self.name = "Jacob"
                self.future = true
            }
        }

        pub resource interface IScores {
            pub var score: Int
            pub var name: String
            pub fun changeScore(_score: Int): Int
        }

        pub fun withInterface() {
            let tempScore: @Scores{IScores} <- create Scores()
            let tempFuture: @Scores{IScores} <- create Scores()
            tempScore.changeScore(_score: 1)
            tempFuture.changeFuture(_future: false) //ERROR
            log(tempFuture.future)
            log(tempScore.score)
            destroy tempFuture
            destroy tempScore

        }

        pub fun withoutInterface() {
            let tempScore: @Scores <- create Scores()
            let tempFuture: @Scores <- create Scores()
            tempScore.changeScore(_score: 1)
            tempFuture.changeFuture(_future: false)
            log(tempScore.score) 
            log(tempFuture.future)
            destroy tempScore
            destroy tempFuture
        }

        init() {
        }

    }

*3. How would we fix this code?*

    pub contract Stuff {

        pub struct interface ITest {
          pub var greeting: String
          //pub var favouriteFruit: String // NO "favouriteFruit" VARIABLE IN STRUCTURE
          pub fun changeGreeting(newGreeting: String): String // ADD FUNCTION TO INTERACT WITH
        }

        // ERROR:
        // `structure Stuff.Test does not conform 
        // to structure interface Stuff.ITest`
        pub struct Test: ITest {
          pub var greeting: String

          pub fun changeGreeting(newGreeting: String): String {
            self.greeting = newGreeting
            return self.greeting // returns the new greeting
          }

          init() {
            self.greeting = "Hello!"
          }
        }

        pub fun fixThis() {
          let test: Test{ITest} = Test()
          let newGreeting = test.changeGreeting(newGreeting: "Bonjour!") // ERROR HERE: `member of restricted type is not accessible: changeGreeting`
          log(newGreeting)
        }
    }
 
