1. Data Collection Method

Collected transaction history from the Compound V2 protocol using the Data_extractor.ipynb file.

Used the Etherscan API to fetch on-chain transactions for each wallet address.

Focused on relevant Compound protocol actions such as deposit, borrow, repay, redeemUnderlying, and liquidationCall.

Extracted and stored full raw transactions in compound_transactions_all.json, and filtered only meaningful actions in compound_transactions_filtered.json.


2. Feature Selection Rationale
For each wallet, the following features were generated from the transaction history:

Feature Name	Description
total_transactions	Total number of Compound-related transactions.
deposit_count	Number of times the wallet made deposits.
borrow_count	Number of times the wallet borrowed funds.
repay_count	Number of repayments made.
redeem_count	Number of withdrawal (redeemUnderlying) actions.
liquidation_count	Number of liquidation events — a strong negative risk indicator.
total_deposit_amount	Total deposited amount (converted from Wei to ETH).
total_borrow_amount	Total borrowed amount (in ETH).

These features provide insight into the wallet's lending and borrowing behavior.


3. Risk Scoring Method
Loaded the ML model trained in Assignment 1.

Applied the model to the features extracted from each wallet.

The output was a predicted_score between 0 and 1000, representing the wallet's risk level.

Higher scores indicate lower-risk (more responsible) wallets, while lower scores reflect riskier behavior.


4. Justification of Risk Indicators
Liquidation events are a direct indicator of default risk.

High borrowing with low repayment indicates higher exposure to debt.

Frequent repayments reflect good debt management.

Active deposits and total volume demonstrate protocol engagement and experience.




Tried with these Methods but failed to collect data:
Metamask Developer API:

Only supports wallet and transaction signing functionality.

Does not provide protocol-level DeFi data like lending or borrowing.

Crossmint and Wallet SDKs:

Primarily built for NFTs and wallet onboarding.

Not relevant for extracting DeFi activity like loans or deposits.

Other GraphQL Queries & Endpoints:

Encountered errors due to deprecated or removed fields, such as:

Fields like "action", "txHash", or even types like "Deposit" were no longer part of the subgraph schema.

Some subgraph endpoints were outdated or broken, showing errors like:

"Page lost in space" (meaning the subgraph is no longer hosted).

Also received GraphQL warnings:

"The Graph will soon start returning validation errors...", indicating upcoming strict schema checks.




Final Working Method:
Data collected using the Python script (Data_extractor.ipynb) by querying The Graph’s hosted Aave V2 subgraph.

Queried wallet-level DeFi activity such as deposit, borrow, and repay using GraphQL with wallet addresses.

This method worked because the subgraph was well-maintained, schema was known, and responses were consistent.

