*1. Explain why standards can be beneficial to the Flow ecosystem.*

    They help app-makers to concentrate on sophistication of functionality rather then taking into account all options for interaction with contracts

*2. What is YOUR favourite food?*

     self.favouriteFood = "Chicken with Potatoes in the Oven"

*3. Please fix this code (Hint: There are two things wrong):*

*The contract interface:*

    pub contract interface ITest {
      pub var number: Int

      pub fun updateNumber(newNumber: Int) {
        pre {
          newNumber >= 0: "We don't like negative numbers for some reason. We're mean."
        }
        post {
          self.number == newNumber: "Didn't update the number to be the new number."
        }
      }

      pub resource interface IStuff {
        pub var favouriteActivity: String
      }

      pub resource Stuff {
        pub var favouriteActivity: String
      }
    }


The implementing contract:

    import ITest from 0x01 // HERE You need to import interface contract

    pub contract Test {
      pub var number: Int

      pub fun updateNumber(newNumber: Int) {
        self.number = 5
      }

      pub resource interface IStuff {
        pub var favouriteActivity: String
      }

      pub resource Stuff: ITest.IStuff {  // HERE You need to specify a contract containing the interface
        pub var favouriteActivity: String

        init() {
          self.favouriteActivity = "Playing League of Legends."
        }
      }

      init() {
        self.number = 0
      }
    }
