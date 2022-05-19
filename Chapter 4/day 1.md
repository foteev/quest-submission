*1. Explain what lives inside of an account.*

  Contracts and Data

*2. What is the difference between the /storage/, /public/, and /private/ paths?*

  /Storage/ is like warehouse, /public/ is muzeum and /private/ is apartaments 

*3. What does .save() do? What does .load() do? What does .borrow() do?*

.save is putting something in /storage/, /load/ is finding it in /storage/ and taking it off, .borrow is shoing something from /storage/ if allowed

*4. Explain why we couldn't save something to our account storage inside of a script.*

We must get rights first to put in, so we can not do it within read-only script 

*5. Explain why I couldn't save something to your account.*

You should sign the transaction with MY access rights, so it is not impossible, but very hard, I think

*Define a contract that returns a resource that has at least 1 field in it. Then, write 2 transactions:
    pub contract Quest {

        pub resource Something {
            pub var piece: String
            init() {
                self.piece = "Secret"
            }
        }

        pub fun createSomething(): @Something {
            return <- create Something()
        }
    }

*A transaction that first saves the resource to account storage, then loads it out of account storage, logs a field inside the resource, and destroys it.*

    import Quest from 0x01

    transaction() {
      prepare(signer: AuthAccount) {

        let saveResource <- Quest.createSomething()
        signer.save(<- saveResource, to: /storage/TestResource)
        let loadResource <- signer.load<@Quest.Something>(from: /storage/TestResource) ?? panic("Empty")
        log(loadResource.piece)
        destroy loadResource
      }

      execute {
      }
    }

*A transaction that first saves the resource to account storage, then borrows a reference to it, and logs a field inside the resource.*

    import Quest from 0x01

    transaction() {
      prepare(signer: AuthAccount) {

        let saveResource <- Quest.createSomething()
        signer.save(<- saveResource, to: /storage/TestResource)
        let loadResource = signer.borrow<&Quest.Something>(from: /storage/TestResource) ?? panic("Empty")
        log(loadResource.piece)
      }

      execute {
      }
    }
