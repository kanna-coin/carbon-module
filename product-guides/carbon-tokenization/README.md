# 🛠️ Carbon Tokenization

Kanna's carbon certification is based on the data origin of the NFT generated in the decentralized audit, stemming from the Polygon blockchain network.

Our application will read the NFT data, check the feasibility for generating a new credit, and if validated, it will generate the carbon tokens on the Solana network, which can then use the Bridge for their use on Web2 or Web3.

<figure><img src="../../.gitbook/assets/Screenshot 2024-04-07 at 12.20.24.png" alt=""><figcaption></figcaption></figure>

{% hint style="warning" %}
The process of tokenizing carbon credits is under development and has not yet been implemented. This page explains how it will work.
{% endhint %}

<details>

<summary>Step1: Validate Process</summary>

In this phase, the application carries out a set of critical checks:

The application begins by searching for the NFT certificate on the Polygon network to check if the wallet that requested tokenization has the necessary authorization. If the wallet has the required permission, the application then confirms if the NFT has a positive carbon balance and whether it has not been used to generate new carbon credits yet. These checks ensure that the carbon credit is valid and available for tokenization.

```rust
// Validation function within the smart contract
pub fn validate_nft_permissions_and_balance(nft_data: &NFTData, user_wallet: &Pubkey) -> Result<()> {
    // Check if the user's wallet owns the ESG audit NFT
    if !nft_data.owner.equals(user_wallet) {
        return Err(ErrorCode::UnauthorizedWallet.into());
    }

    // Check if the carbon balance is positive
    if nft_data.carbon_balance <= 0 {
        return Err(ErrorCode::NegativeCarbonBalance.into());
    }

    // Check if the NFT has already been used for minting credits
    if nft_data.is_already_minted {
        return Err(ErrorCode::NftAlreadyUsedForMinting.into());
    }

    Ok(())
}
```

</details>

<details>

<summary>Step 2: Mint Tokens</summary>

After successful validation, the carbon credit is tokenized on Solana Blockchain:

With the validated data, an NFT is minted on the Solana network, containing all the origin information and the Kanna ESG certification. This NFT represents the validated carbon credit

```rust
#[account]
pub struct CarbonCredit {
    pub hash: String,
    pub quantity_co2: u64,
    pub emission_date: i64,
    pub validity: i64,
    pub generating_projects: Vec<String>,
    pub esg_certifications: Vec<EsgCertification>,
    pub additional_certifications: Vec<AdditionalCertification>,
}

#[derive(AnchorSerialize, AnchorDeserialize, Clone)]
pub struct EsgCertification {
    pub audit_nft_link: String,
    pub environmental_score: u8,
    pub social_score: u8,
    pub governance_score: u8,
    pub carbon_balance: u64,
}

#[derive(AnchorSerialize, AnchorDeserialize, Clone)]
pub struct AdditionalCertification {
    pub certifier_name: String,
    pub public_audit_link: String,
}
```

</details>

<details>

<summary>Step 3: Utility</summary>

Finally, the carbon credit NFT serves as a multifunctional asset:

The tokenized carbon credit can be used in various ways in our economy, facilitating the transition between web 2 and web3. It can be used to offset carbon emissions, traded in specialized markets, or used to support sustainable practices.

To ensure the uniqueness and backing of the asset, the carbon credit NFT is locked in a specific pool. This prevents the duplication of credit use and creates a solid foundation for the asset's digital value.

</details>
