# How to Tokenize a Real-World Asset in the Stellar Network

To tokenize a real-world asset (RWA) on the Stellar network, you must first set up an issuing account to create the asset and a distribution account to hold it. Next, the distribution account needs to establish a trustline for the asset, which involves getting approval from the issuer. Finally, you transfer the asset from the issuing account to the distribution account using a payment operation.

## Step 1: Create Accounts

*   **Issuing account:** This is the primary account that will issue the digital representation of your RWA. You will need a Stellar account and a private key to access it.
*   **Distribution account:** This is a secondary account that will receive and hold the new asset.

## Step 2: Establish the Trustline

The distribution account must establish a trustline to the asset being issued by the primary account.

*   This process involves the distribution account sending an operation to the network, indicating it trusts the issuer to provide the asset.
*   The issuer must approve the distribution account to hold the asset, as specified by the trustline.

## Step 3: Transfer the Asset

Once the trustline is established, you can send the asset from the issuing account to the distribution account.

*   This is done through a payment operation, which is the fundamental building block of transactions on the Stellar network.
*   The transaction records the transfer of the digital representation of your RWA to the new account.

## Other Considerations

*   **Legal and compliance:** Before beginning, you must consider the legal and regulatory requirements for your specific RWA in your target jurisdictions. This includes KYC/AML and potentially securities laws, as outlined by Stellar and other sources.
*   **Anchors:** Anchors are organizations that help convert real-world value to digital assets on Stellar, such as fiat currency or digital funds.
*   **Asset information:** You can publish information about the asset on your stellar.toml file to provide users with details about the token.

---

# Tokenizing a Real World Asset (RWA) on the Stellar Network

Tokenizing a Real World Asset (RWA) on the Stellar network involves creating a digital representation of that asset on the blockchain. Unlike other blockchains, Stellar has built-in features for tokenization and can be done without writing a custom smart contract. Stellar also offers its new Soroban smart contract platform for more advanced tokenization with complex logic.

Regardless of the method, the process fundamentally requires a robust off-chain component to manage the legal ownership and custodial aspects of the underlying RWA.

## The Off-Chain Component

This crucial first step ensures the digital token is legally tied to the physical asset.

*   **Legal framework:** Work with legal teams in relevant jurisdictions to determine applicable regulations for the RWA (e.g., securities, real estate, commodities).
*   **Custody and verification:** Establish secure, transparent procedures for the custody of the physical asset. This can involve third-party audits and the use of services from custodians like BitGo or Fireblocks, which are integrated with the Stellar ecosystem.
*   **Documentation:** Create the necessary legal documents that define the rights and obligations of the token holder, such as ownership stakes, dividend payments, or redemption rights.

## The On-Chain Component (Classic Stellar Assets)

For a straightforward tokenization, you can use Stellar's native asset-issuance protocols without writing any code.

### Step 1: Create Accounts

You need to set up two separate Stellar accounts:

*   **Issuer account:** This account "issues" and holds the initial supply of the new asset.
*   **Distribution account:** This account receives the tokens from the issuer and distributes them to investors.

### Step 2: Establish a Trustline

Before any account can hold your new asset, it must "trust" the issuer. This step involves submitting a Change Trust operation to the network.

*   The distribution account sends a transaction to create a trustline with the issuer account.
*   This transaction specifies the asset code (the token name, e.g., "REALESTATE") and the issuer's public key.

### Step 3: Issue the Asset

On Stellar, issuing an asset is done by sending a payment.

*   The issuer account sends a Payment operation to the distribution account.
*   The payment's asset is your new token, and the amount is the number of tokens you want to create.

### Step 4: Add Compliance Flags (Optional, but Recommended for RWAs)

For many regulated assets, Stellar provides built-in features to enforce compliance. The issuer can set asset flags on the issuing account.

*   **Authorization required:** You can require users to be explicitly approved before they can hold and trade your asset.
*   **Clawback enabled:** This allows the issuer to take back assets in case of regulatory or legal issues.
*   **Asset freeze:** You can freeze specific accounts from holding, trading, and sending the token.

## The On-Chain Component (Soroban Smart Contracts)

For tokenizing complex RWAs that require more advanced logic (e.g., automated distributions, special vesting schedules), you can use Soroban, Stellar's smart contract platform.

### Step 1: Deploy a Contract Token

Instead of using Stellar's native assets, you can create and manage tokens directly with a Soroban smart contract. The contract will define all the token's logic.

### Step 2: Implement a Stellar Asset Contract (SAC)

Stellar's built-in assets can be wrapped into a Soroban contract via the Stellar Asset Contract (SAC), allowing them to interact with more complex smart contract applications.

## Example: Tokenizing a Commercial Property

*   **Off-chain:** A company, "Property LLC," legally owns a building. They work with a lawyer to draft a contract that assigns token holders a share in the property's rental income.
*   **On-chain (Classic Asset):**
    *   Property LLC creates an issuer account and a distribution account on Stellar.
    *   Investors create Stellar accounts and establish a trustline with Property LLC's issuer account.
    *   Property LLC issues 1 million "COMMERCIALPROP" tokens by sending a payment from its issuer account to its distribution account.
    *   Property LLC sets the "authorization required" flag so only approved, vetted investors can hold the token.
*   **On-chain (Soroban):** Property LLC could also use Soroban to create a custom smart contract that automatically distributes rental income to token holders on a monthly basis.
