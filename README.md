# 1. Market Contract Documentation
Path: contracts/src/market

## Overview
The Market contract is an ink! smart contract designed for decentralized trading on the Aleph zero network. It enables users to open, close, and manage leveraged positions in a market using a specified collateral asset. The contract integrates with the AZEX for obtaining price data and supports the use of a wrapped native asset (WAZERO) as collateral.

## Key Features and Functions

### Contract Structure

The contract is organized into several modules:

- **errors**: Defines custom error types used within the contract.
- **position**: Defines the Position struct representing an open position in the market.
- **market**: The main module defining the Market smart contract.

### Storage

- **data**: Storage for PSP22 token data (total supply, balances, allowances, etc.).
- **name**, **symbol**, **decimals**: Market metadata for PSP22 token.
- **owner**: AccountId of the contract owner.
- **positions**: Mapping of user and position ID to Position details.
- **ids_per_user**: Mapping of user to a vector of position IDs.
- **new_id**: Mapping of user to the latest position ID.
- **underlying_asset**, **oracle**, **vault**, **wazero**: AccountIds of associated contracts.
- **liquidation_threshold**, **liquidation_penalty**, **protocol_fee**: Parameters for liquidation and fee calculations.

### Constructors

1. **default**: Creates a new instance of the Market contract with default values.
2. **new**: Initializes the Market contract with user-provided parameters.

### View Functions

1. **view_market_data**: Retrieves market metadata (name, symbol, decimals).
2. **view_underlying_asset**: Retrieves the AccountId of the underlying asset.
3. **view_position**: Retrieves details of a specific position.
4. **view_positions**: Retrieves all positions for a given user.

### Internal Functions

1. **get_price**: Retrieves the price of a specified symbol from the DIA price oracle.
2. **get_symbol_and_decimals**: Retrieves symbol and decimals information for a given token.
3. **calculate_usd_from_asset_amount**: Converts asset amount to USD based on price and decimals.
4. **calculate_pnl_percent**: Calculates the percentage profit/loss for a position.
5. **calculate_asset_amount_from_usd**: Converts USD amount to asset amount based on price and decimals.
6. **wrap_native**: Wraps native asset (WAZERO) into the ERC-20 format.
7. **calculate_amount_and_mint**: Calculates and mints PSP22 tokens based on the deposited amount.
8. **burn_and_calculate_amount**: Burns PSP22 tokens and calculates the corresponding asset amount.
9. **open_position**: Opens a new leveraged position in the market.
10. ... (other internal functions for deposit, withdrawal, liquidation, etc.)

### Public Functions

1. **deposit_native**: Deposits native asset (WAZERO) into the contract.
2. **deposit**: Deposits a specified amount of the underlying asset into the contract.
3. **withdraw_native**: Withdraws native asset (WAZERO) from the contract.
4. **withdraw**: Withdraws a specified amount of the underlying asset from the contract.
5. **open_native**: Opens a new leveraged position using native asset (WAZERO) as collateral.
6. **open**: Opens a new leveraged position using a specified collateral asset.
7. **close**: Closes an open position and handles profit/loss accordingly.
8. **is_liquidatable**: Checks if a position is eligible for liquidation.
9. **liquidate**: Liquidates a position, seizing collateral and applying fees.

### PSP22 Interface Implementation

The Market contract implements the PSP22 interface for ERC-20 compatibility. This includes functions for transferring tokens, checking balances, allowances, and approval mechanisms.

### PSP22Metadata Interface Implementation

The Market contract also implements the PSP22Metadata interface, providing functions to query metadata such as token name, symbol, and decimals.

## Usage

The Market contract is designed for deployment and usage on the network, facilitating decentralized trading. Users can interact with the contract to open, close, and manage leveraged positions in a market. It supports both native asset (WAZERO) and other specified collateral assets.

# 2. Vault Contract Documentation

## Overview

The Vault contract is an ink! smart contract designed to manage collateral assets for leveraged trading positions in the Acala network. It allows users to deposit and withdraw collateral, supporting multiple collateral assets and markets.

## Features and Functions

### Contract Structure

The contract is organized into modules:

- `errors`: Defines custom error types used within the contract.
- `traits`: Contains the CollateralVault trait defining the interface.
- `vault`: The main module defining the Vault smart contract.

### Storage

- `admin`: AccountId of the contract administrator.
- `balances`: Mapping of (market, user, position) to (balance, collateral asset).
- `markets`: List of market AccountIds.
- `assets`: List of supported collateral asset AccountIds.

### Constructors

- `new`: Creates a new instance of the Vault contract with default values.

### CollateralVault Trait Functions

1. **`user_collateral`**
   - **Description**: Retrieves the collateral balance and asset for a specific user's position in a market.
   - **Parameters**: `market` (AccountId), `user` (AccountId), `id` (u128).
   - **Returns**: Option<(Balance, AccountId)>.

2. **`supported_collateral_assets`**
   - **Description**: Retrieves the list of supported collateral asset AccountIds.
   - **Returns**: Vec<AccountId>.

3. **`markets_with_access`**
   - **Description**: Retrieves the list of markets that the contract has access to.
   - **Returns**: Vec<AccountId>.

4. **`deposit`**
   - **Description**: Deposits collateral into a user's position.
   - **Parameters**: `user` (AccountId), `id` (u128), `collateral_asset` (AccountId), `collateral_amount` (Balance).
   - **Returns**: Result<(), VaultError>.

5. **`withdraw`**
   - **Description**: Withdraws collateral from a user's position.
   - **Parameters**: `user` (AccountId), `id` (u128), `withdraw_amount` (Balance), `receiver` (AccountId).
   - **Returns**: Result<(), VaultError>.

6. **`add_asset`**
   - **Description**: Adds a new collateral asset to the supported list.
   - **Parameters**: `collateral_asset` (AccountId).
   - **Returns**: Result<(), VaultError>.

7. **`add_market`**
   - **Description**: Adds a new market to the list of accessible markets.
   - **Parameters**: `market` (AccountId).
   - **Returns**: Result<(), VaultError>.

## Usage

The Vault contract is designed to be deployed and used on the Acala network for managing collateral assets in decentralized trading. Users can interact with the contract to deposit and withdraw collateral for their leveraged positions. The contract supports multiple collateral assets and markets.

## End-to-End Tests

The contract includes end-to-end tests covering essential functionalities:

1. **`add_asset_works`**
   - Covers adding a new collateral asset to the Vault.

2. **`add_market_works`**
   - Covers adding a new market to the Vault.

3. **`deposit_works`**
   - Covers the deposit of collateral into the Vault, including checks for markets and assets.

4. **`withdraw_works`**
   - Covers the withdrawal of collateral from the Vault, including checks for markets, assets, and sufficient balances.

## Development Environment

- Ensure that the necessary dependencies are installed.
- Use the provided `Vault` smart contract in your ink! development environment.
- Follow the provided tests for end-to-end testing and validation.

For detailed instructions on setting up the development environment and deploying the contract, refer to the corresponding development and deployment guides.