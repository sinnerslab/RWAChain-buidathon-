
# RWACHAIN

This repository contains smart contracts for an advanced NFT-based land ownership and rental system with an integrated marketplace. The system allows for tokenization of land ownership, facilitates various trading mechanisms, and supports temporary transfers of usage rights.

## Contracts

### 1. IERC4907

**Purpose:** This interface extends the ERC721 standard to include user and expires functionality, enabling time-limited permissions on NFTs. It's crucial for implementing rental or time-limited usage features for NFTs.

**Key Functions:**
- `setUser(uint256 tokenId, address user, uint64 expires)`: Assigns user rights to an address for a specific NFT with an expiration time.
- `userOf(uint256 tokenId)`: Retrieves the current user of an NFT.
- `userExpires(uint256 tokenId)`: Returns the expiration timestamp of the current user's rights.

**Events:**
- `UpdateUser(uint256 indexed tokenId, address indexed user, uint64 expires)`: Emitted when user rights are updated.

### 2. RWAChain 0xd68CCa05D2b750329A125f2CBc00b97e3FbF53D4

**Purpose:** This is the core NFT contract representing land ownership as non-fungible tokens. It implements ERC721 and IERC4907 standards, allowing for ownership and time-limited usage rights.

**Key Features:**
- ERC721 Compatibility: Implements standard NFT functions like `transfer`, `approve`, etc.
- Minting: Allows creation of new land NFTs with associated metadata.
- User Rights Management: Implements IERC4907 for managing temporary usage rights.
- Supply Control: Includes a maximum supply limit and tracking of total supply.
- Metadata Management: Uses ERC721URIStorage for flexible metadata handling.

**Key Functions:**
- `mint(string calldata MtokenURI)`: Mints a new land NFT with specified metadata.
- `setUser(uint256 tokenId, address user, uint64 expires)`: Sets temporary user rights for an NFT.
- `userOf(uint256 tokenId)`: Returns the current user of an NFT.
- `userExpires(uint256 tokenId)`: Returns the expiration time of user rights.
- `setMaxSupply(uint256 maxSupply_)`: Allows owner to adjust the maximum supply.

**Security Measures:**
- Access Control: Uses OpenZeppelin's `Ownable` for restricted functions.
- Checks: Implements various require statements to ensure valid operations.

### 3. Marketplace 0xf6489F4f8BC803c2260530E40645f977ecE839D9

**Purpose:** This contract provides a comprehensive platform for trading and renting RWAChain NFTs through various mechanisms including auctions, direct sales, and rentals.

**Key Features:**
1. Auctions
   - Creation: Sellers can create time-limited auctions for their NFTs.
   - Bidding: Users can place bids on active auctions.
   - Automatic Extension: Auctions are extended if a bid is placed near the end time.
   - Settlement: Auctions can be settled after the end time, transferring the NFT and funds.

2. Direct Sales
   - Listing: Sellers can list their NFTs for a fixed price.
   - Purchasing: Buyers can directly purchase listed NFTs.
   - Cancellation: Sellers can cancel their listings.

3. Rentals
   - Listing: NFT owners can list their tokens for rent with a price and duration.
   - Renting: Users can rent NFTs, gaining temporary usage rights.
   - Cancellation: Owners can cancel their rental listings.

4. Financial Management
   - Platform Fees: Implements a percentage-based fee on all transactions.
   - Withdrawals: Users can withdraw funds from outbid or cancelled transactions.
   - Owner Withdrawals: Contract owner can withdraw accumulated platform fees.

**Key Functions:**
- Auction Functions: `createAuction`, `placeBid`, `endAuction`
- Direct Sale Functions: `createDirectSale`, `buyDirectSale`, `cancelDirectSale`
- Rental Functions: `createRentSale`, `rentNFT`, `cancelRentSale`
- Financial Functions: `withdraw`, `withdrawPlatformFees`
- Management Functions: `setNftContract`, `setPlatformFee`, `pause`, `unpause`

**Security Measures:**
- Reentrancy Guard: Uses OpenZeppelin's `ReentrancyGuard` to prevent reentrancy attacks.
- Pausability: Implements `Pausable` for emergency stops.
- Access Control: Utilizes `Ownable` for restricted functions.
- Checks-Effects-Interactions Pattern: Follows best practices to prevent potential vulnerabilities.

## System Workflow

1. NFT Creation: Users mint land NFTs using the `RWAChain` contract.
2. Marketplace Interaction: NFT owners can list their tokens on the `Marketplace` contract.
3. Trading Options:
   - Sellers can create auctions, direct sales, or rental listings.
   - Buyers can bid on auctions, purchase directly, or rent NFTs.
4. Transaction Settlement: The marketplace handles transfers of NFTs and funds, including fee calculations.
5. Rental Management: For rented NFTs, the system manages temporary user rights and expirations.

## Security Considerations

- The contracts inherit from OpenZeppelin's audited contracts for enhanced security.
- Reentrancy guards are implemented to prevent common attack vectors.
- The system uses a checks-effects-interactions pattern to ensure state consistency.
- Pausability allows for emergency stops in case of detected issues.
- Access control mechanisms restrict sensitive functions to authorized users only.

## Deployment and Integration

When deploying these contracts:
1. Deploy the `RWAChain` contract first.
2. Deploy the `Marketplace` contract, passing the address of the `RWAChain` contract.
3. Call `setApprovalForAll` on the `RWAChain` contract to approve the `Marketplace` for trading.

## Future Improvements

- Implementation of API of UK Real Estate System for verification of the owner of the property
- Integration with real-world land registry systems for increased utility.
- Addition of fractional ownership capabilities for land NFTs.

This system provides a robust, secure, and flexible solution for tokenizing land ownership, facilitating various trading mechanisms, and managing temporary usage rights, all secured on the base blockchain.