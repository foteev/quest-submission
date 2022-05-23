pub contract CryptoPoops {
  pub var totalSupply: UInt64

  // This is an NFT resource that contains a name,
  // favouriteFood, and luckyNumber
  pub resource NFT {
    pub let id: UInt64

    pub let name: String
    pub let favouriteFood: String
    pub let luckyNumber: Int

    init(_name: String, _favouriteFood: String, _luckyNumber: Int) {
      self.id = self.uuid

      self.name = _name
      self.favouriteFood = _favouriteFood
      self.luckyNumber = _luckyNumber
    }
  }

  // This is a resource interface that allows us to restrict available variables and functions of resource for users
  pub resource interface CollectionPublic {
    pub fun deposit(token: @NFT)
    pub fun getIDs(): [UInt64]
    pub fun borrowNFT(id: UInt64): &NFT
  }

  // This is a container, where NFTs will be stored
  pub resource Collection: CollectionPublic {
    pub var ownedNFTs: @{UInt64: NFT}

  // This is a function inside the resource to deposit NFT(add them to container)
    pub fun deposit(token: @NFT) {
      self.ownedNFTs[token.id] <-! token
    }

  // This is a function inside the resource to withdraw NFT(remove them from container to delete them or move somewhere else)
    pub fun withdraw(withdrawID: UInt64): @NFT {
      let nft <- self.ownedNFTs.remove(key: withdrawID) 
              ?? panic("This NFT does not exist in this Collection.")
      return <- nft
    }

  // This is a function inside the resource to get unique NFT id's
    pub fun getIDs(): [UInt64] {
      return self.ownedNFTs.keys
    }
  
  // This is a function inside the resource to get NFT's reference to use them, copy or call functions
    pub fun borrowNFT(id: UInt64): &NFT {
      return &self.ownedNFTs[id] as &NFT
    }

    init() {
      self.ownedNFTs <- {}
    }

  // This is a required function inside the resource that contains another resource. It use to destroy "nested" resource.
    destroy() {
      destroy self.ownedNFTs
    }
  }

  // Here is a function to create empty Collection container
  pub fun createEmptyCollection(): @Collection {
    return <- create Collection()
  }

  // This is a resource, that contains function to create NFT, and it is used to restrict NFT-creators by accounts
  pub resource Minter {

  // Yep, the the sacrament of conception takes place here
    pub fun createNFT(name: String, favouriteFood: String, luckyNumber: Int): @NFT {
      return <- create NFT(_name: name, _favouriteFood: favouriteFood, _luckyNumber: luckyNumber)
    }

    pub fun createMinter(): @Minter {
      return <- create Minter()
    }

  }

  init() {
    self.totalSupply = 0
    
  // Here is Minter resource, like the passcard, is assigned to account
    self.account.save(<- create Minter(), to: /storage/Minter)
  }
}
