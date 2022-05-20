*For today's quest, you will be looking at a contract and a script. You will be looking at 4 variables (a, b, c, d) and 3 functions (publicFunc, contractFunc, privateFunc) defined in SomeContract. In each AREA (1, 2, 3, and 4), I want you to do the following: for each variable (a, b, c, and d), tell me in which areas they can be read (read scope) and which areas they can be modified (write scope). For each function (publicFunc, contractFunc, and privateFunc), simply tell me where they can be called.*

    access(all) contract SomeContract {
        pub var testStruct: SomeStruct

        pub struct SomeStruct {

            //
            // 4 Variables
            //

            pub(set) var a: String

            pub var b: String

            access(contract) var c: String

            access(self) var d: String

            //
            // 3 Functions
            //

            pub fun publicFunc() {}

            access(contract) fun contractFunc() {}

            access(self) fun privateFunc() {}


            pub fun structFunc() {
                /**************/  // VAR'S "a" "b" "c" "d" CAN BE READ/WRITE
                /*** AREA 1 ***/  // FUN'S "publicFunc" "contractFunc" "privateFunc" CAN BE CALLED
                /**************/
            }

            init() {
                self.a = "a"
                self.b = "b"
                self.c = "c"
                self.d = "d"
            }
        }

        pub resource SomeResource {
            pub var e: Int

            pub fun resourceFunc() {
                /**************/  // VAR'S "a" CAN BE READ/WRITE, "b" "c" CAN BE READ
                /*** AREA 2 ***/  // FUN'S "publicFunc" "contractFunc" CAN BE CALLED
                /**************/
            }

            init() {
                self.e = 17
            }
        }

        pub fun createSomeResource(): @SomeResource {
            return <- create SomeResource()
        }

        pub fun questsAreFun() {
            /**************/    // VAR'S "a" CAN BE READ/WRITE, "b" "c" CAN BE READ
            /*** AREA 3 ****/   // FUN'S "publicFunc" "contractFunc" CAN BE CALLED
            /**************/
        }

        init() {
            self.testStruct = SomeStruct()
        }
    }
    
Script:

    import SomeContract from 0x01

    pub fun main() {
      /**************/    // VAR's "a" "b" CAN BE READ
      /*** AREA 4 ***/    // FUN "publicFunc" CAN BE CALLED
      /**************/
    }
