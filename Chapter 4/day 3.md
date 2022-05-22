*1. Why did we add a Collection to this contract? List the two main reasons.*

    To automize process of NFT's allocation and to get possibility to use interface restrictions.

*2. What do you have to do if you have resources "nested" inside of another resource? ("Nested resources")*

    I need to declare destroy() function for child resources

*3. Brainstorm some extra things we may want to add to this contract. Think about what might be problematic with this contract and how we could fix it.*

*Idea #1: Do we really want everyone to be able to mint an NFT? 🤔.*

    It can violate the uniqueness of the collection, may be, or exceed the volume of account storage.

*Idea #2: If we want to read information about our NFTs inside our Collection, right now we have to take it out of the Collection to do so. Is this good?*

    No, it will be better to use read-only references.

*Idea 3* 

    We can add NFT customization in relation to depositor account, make ID's more readable and clear.



