*1. What does "force casting" with as! do? Why is it useful in our Collection?*
    
    We can check if token is @NFT type, otherwise 'panic'

*2. What does auth do? When do we use it?*

    auth downcasts a higher level interface restriction (in our case contract interface) to lower (resource interface)

*3. Contract:*

    import NonFungibleToken from 0x02
    pub contract CryptoPoops: NonFungibleToken {
      pub var totalSupply: UInt64

     //stuff here...
     
      pub resource NFT: NonFungibleToken.INFT {
        pub let id: UInt64

        //stuff here...
      }

      pub resource interface CollectionAuth {
        pub fun borrowAuthNFT(id: UInt64): &NFT
      }

      pub resource Collection: NonFungibleToken.Provider, NonFungibleToken.Receiver, NonFungibleToken.CollectionPublic, CollectionAuth {
        pub var ownedNFTs: @{UInt64: NonFungibleToken.NFT}

         //stuff here...
         
        pub fun borrowAuthNFT(id: UInt64): &NFT {
          let ref = &self.ownedNFTs[id] as auth &NonFungibleToken.NFT
          return ref as! &NFT
        }
        
         //stuff here...
    } 

*Start Transaction*

    import CryptoPoops from 0x03
    import NonFungibleToken from 0x02

    transaction {

      prepare(acct: AuthAccount) {

      acct.save(<- CryptoPoops.createEmptyCollection(), to: /storage/Collection1)


      acct.link<&CryptoPoops.Collection{NonFungibleToken.Provider, NonFungibleToken.Receiver, 
                                          NonFungibleToken.CollectionPublic, CryptoPoops.CollectionAuth}>
                                            (/public/Collection1, target: /storage/Collection1)
      }

      execute {
        log("Collection created")
      }
    }

*Mint NFT Transaction*

    import CryptoPoops from 0x03
    import NonFungibleToken from 0x02

    transaction(name: String, favouriteFood: String, luckyNumber: Int, recipient: Address) {

        prepare(admin: AuthAccount) {
            let minterRef = admin.borrow<&CryptoPoops.Minter>(from: /storage/Minter)
                                ?? panic("It is not Minter acc!")
            let nft <- minterRef.createNFT(name: name, favouriteFood: favouriteFood, luckyNumber: luckyNumber)

            let CollectionRef = getAccount(recipient).getCapability(/public/Collection1)
                                    .borrow<&CryptoPoops.Collection{NonFungibleToken.Receiver}>()
                                        ?? panic("There is no Collection!")

            log(nft.id)

            CollectionRef.deposit(token: <- nft)
        }

        execute {
            log("NFT deposit is done!")
        }
    }
    
 *Read Metadata Script*

    import CryptoPoops from 0x03

    pub fun main(address: Address, id: UInt64): String {
      let ref = getAccount(address).getCapability<&CryptoPoops.Collection{CryptoPoops.CollectionAuth}>(/public/Collection1)
                .borrow() ?? panic("No Collection!")

      let nftRef: &CryptoPoops.NFT = ref.borrowAuthNFT(id: id)
      return nftRef.name

    }
