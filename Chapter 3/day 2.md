https://play.onflow.org/b678d256-b144-46ba-ae22-9760cb5cf192?type=account&id=42a0b954-fcff-4e28-b332-362067127671&storage=none
    
    
    pub contract Quest {

        pub var codesArray: @[CodeA]
        pub var codesDictionary: @{Int: CodeB}

        pub resource CodeA {
            pub let codeA: Int

            init() {
                self.codeA = 42
            }
        }

        pub resource CodeB {
            pub let codeB: Int

            init() {
                self.codeB = 43
            }
        }

        init() {
            self.codesArray <- []
            self.codesDictionary <- {}
        }

        pub fun addArray(input: @CodeA) {
            self.codesArray.append(<- input)
        }

        pub fun addDictionary(input: @CodeB) {
            let key = input.codeB
            let tempDictionary <- self.codesDictionary[key] <- input
            destroy tempDictionary
        }

        pub fun removeArray(index: Int): @CodeA {
            return <- self.codesArray.remove(at: index)
        }

        pub fun removeDictionary(key: Int): @CodeB {
            let code <- self.codesDictionary.remove(key: key) ?? panic("empty")
            return <- code
        }
    }
