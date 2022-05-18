  https://play.onflow.org/3eaf0e3f-67de-4a7b-8dbe-39259b0c2ae3?type=script&id=c07dc5fa-ba61-4367-aef7-9d2e2f4fec51&storage=none
  
  Contract:
      
    pub contract Quest {

        pub var dictionary: @{String: Limits}

        pub resource Limits {
            pub let limit: Int64

            init(_limit: Int64) {
                self.limit = _limit
            }
        }

        init() {
            self.dictionary <- {
                "Jacob": <- create Limits(_limit: 128),
                "Ian": <- create Limits(_limit: 256)
            }
        }

        pub fun getLimit(key: String): &Limits {
            return &self.dictionary[key] as &Limits
        }
    }
    
Script:

    import Quest from 0x01

    pub fun main(): Int64 {
      let refLimit = Quest.getLimit(key: "Ian")
      let refLimit2 = Quest.getLimit(key: "Jacob")
      log("Limits summary:") 
      return refLimit.limit + refLimit2.limit
    }

*Explain, in your own words, why references can be useful in Cadence.*

We can read and use values from the resource without interacting, so it can be useful in statistics or read-only data structures.
