*1. Describe what an event is, and why it might be useful to a client.*

  Event is some value, that emits when it was intended in contract. It uses to broadcast information about some changes. 

*2. Deploy a contract with an event in it, and emit the event somewhere else in the contract indicating that it happened.*
*3. Using the contract in step 2), add some pre conditions and post conditions to your contract to get used to writing them out.*

        pub contract Test {

            pub event LotteryDraw(_money: Int256)

            pub resource interface ILottery {
                pub var money: Int256
                pub fun updateMoney(): Int256
            }


            pub resource LotteryResource: ILottery {
                pub var money: Int256

                pub fun updateMoney(): Int256 {  

                    post {
                      result < 1000000: "You become a millionaire!"
                    }

                    self.money = self.money*2
                    emit LotteryDraw(_money: self.money)
                   return self.money 
                }

                init() {
                    self.money = 1
                }
            }

            pub fun createResource(): @LotteryResource {
                let resource <- create LotteryResource()
                return <- resource
            }
        }

Transaction:

    import Test from 0x02

    transaction() {

      prepare(signer: AuthAccount) {

        signer.save(<- Test.createResource(), to: /storage/MyResource)
        signer.link<&Test.LotteryResource{Test.ILottery}>(/public/MyResource, target: /storage/MyResource)

        log("Lottery  started")
      }
    }

Script:

    import Test from 0x02

    pub fun main() {

        let lotteryResult = getAccount(0x02).getCapability<&Test.LotteryResource{Test.ILottery}>(/public/MyResource)
                                .borrow() ?? panic("The drawing was not carried out")

        while lotteryResult.money < 10000001 {

            lotteryResult.updateMoney()
            log(lotteryResult.money)
        }
    }

*4. For each of the functions below (numberOne, numberTwo, numberThree), follow the instructions.*

    pub contract Test {

      // YES
      // Tell me whether or not this function will log the name.
      // name: 'Jacob'
      pub fun numberOne(name: String) {
        pre {
          name.length == 5: "This name is not cool enough."
        }
        log(name)
      }

      // YES
      // Tell me whether or not this function will return a value.
      // name: 'Jacob'
      pub fun numberTwo(name: String): String {
        pre {
          name.length >= 0: "You must input a valid name."
        }
        post {
          result == "Jacob Tucker"
        }
        return name.concat(" Tucker")
      }

      pub resource TestResource {
        pub var number: Int

        // NO - POST CONDITION FAILED, VALUE of `self.number` is '1'
        // Tell me whether or not this function will log the updated number.
        // Also, tell me the value of `self.number` after it's run.
        pub fun numberThree(): Int {
          post {
            before(self.number) == result + 1
          }
          self.number = self.number + 1
          return self.number
        }

        init() {
          self.number = 0
        }

      }

    }
