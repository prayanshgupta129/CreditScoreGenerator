# DeFi Wallet Credit Scoring Model for Aave V2

This project develops a robust machine learning model to assign a credit score between 0 and 1000 to wallets based solely on their historical transaction behavior within the Aave V2 protocol. Higher scores indicate reliable and responsible usage, while lower scores reflect potentially risky, bot-like, or exploitative behavior.

## Deliverables

-   `score_wallets.py`: The core Python script that loads raw transaction data, engineers features, applies the scoring logic, and generates wallet credit scores.
-   `README.md`: This file, providing a comprehensive explanation of the chosen methodology, system architecture, and processing flow.
-   `analysis.md`: A separate file presenting an in-depth analysis of the generated wallet scores, including distribution graphs and insights into the behavior of wallets across different score ranges.

## Methodology Chosen: Heuristic-Based Scoring

Given the absence of pre-labeled data (i.e., explicit "good" or "bad" wallet classifications), this problem is inherently an unsupervised learning challenge. A **heuristic-based scoring system** was selected for this task due to several key advantages:

1.  **Transparency and Interpretability:** The scoring logic is explicit, allowing for a clear understanding of how each transaction behavior contributes to the final score. This is crucial for trust and debugging in financial applications.
2.  **No Labeled Data Requirement:** It bypasses the need for extensive and potentially subjective manual labeling of historical wallet data.
3.  **Direct Control and Domain Expertise Integration:** Domain knowledge about DeFi lending protocols (like Aave V2) can be directly translated into specific feature engineering and scoring rules.
4.  **Extensibility:** The modular nature of heuristic rules allows for easy addition of new features, adjustment of weights, or refinement of existing logic as understanding of DeFi behavior evolves or new data becomes available.

## Architecture and Processing Flow

The credit scoring system is designed as a one-step script that takes raw transaction data as input and produces a CSV file of wallet scores.

```mermaid
graph TD
    A[Raw Aave V2 Transactions (JSON in ZIP)] --> B_Load(Load Data & Initial Preprocessing)
    B_Load -- Cleaned DataFrame --> C_Features{Feature Engineering: Wallet Aggregation}
    C_Features -- Wallet-level Behavioral Features --> D_Scoring(Credit Scoring Logic)
    D_Scoring -- Calculated Scores --> E_Output[Output: wallet_scores.csv]

    subgraph Data Flow
        B_Load -- Rename 'userWallet' to 'wallet' --> B1_Extract(Extract 'amount', 'assetPriceUSD' from 'actionData')
        B1_Extract -- Calculate Transaction USD Value --> B2_Remove(Remove Redundant Columns)
    end
