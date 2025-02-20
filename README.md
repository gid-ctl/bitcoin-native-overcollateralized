# SatoshiStable: Bitcoin-Native Stablecoin Protocol

A secure, transparent, and capital-efficient stablecoin protocol built on Stacks Layer 2, leveraging native Bitcoin as collateral. SatoshiStable enables users to mint USD-pegged stablecoins while maintaining Bitcoin's security guarantees.

## Features

- **Native Bitcoin Collateral**: Deposit BTC directly without wrapped tokens
- **Capital Efficiency**: 150% collateralization ratio
- **Dynamic Liquidation**: Automated position management with 120% liquidation threshold
- **Trustless Price Oracle**: Secure price feeds with validity windows
- **Bitcoin Settlement**: Full compliance with Bitcoin finality guarantees
- **Flash Loan Protection**: Built-in safeguards against price manipulation

## Technical Specifications

### Collateralization Parameters

- Minimum Collateralization Ratio: 150%
- Liquidation Threshold: 120%
- Minimum Deposit: 1,000,000 satoshis
- Price Validity Period: 144 blocks (~1 day)

### Smart Contract Functions

#### User Operations

1. **Deposit Collateral**

   ```clarity
   (deposit-collateral (amount uint))
   ```

   - Deposit BTC as collateral
   - Requires amount ≥ minimum deposit (1M sats)
   - Updates user's collateral position

2. **Mint Stablecoin**

   ```clarity
   (mint-stablecoin (amount uint))
   ```

   - Mint USD-pegged stablecoins
   - Requires valid price feed
   - Maintains minimum collateralization ratio
   - Updates user's debt position

3. **Repay Stablecoin**

   ```clarity
   (repay-stablecoin (amount uint))
   ```

   - Repay outstanding stablecoin debt
   - Reduces user's debt position
   - Updates total supply

4. **Withdraw Collateral**
   ```clarity
   (withdraw-collateral (amount uint))
   ```
   - Withdraw BTC collateral
   - Requires maintaining minimum collateralization
   - Prevents unsafe withdrawals

#### Liquidation Mechanism

```clarity
(liquidate-position (user principal))
```

- Triggers when position falls below 120% collateralization
- Records liquidation history
- Clears user position
- Updates total supply

#### Read-Only Functions

1. **Get Position**

   ```clarity
   (get-position (user principal))
   ```

   - Returns user's current position
   - Shows collateral and debt amounts

2. **Get Collateral Ratio**

   ```clarity
   (get-collateral-ratio (user principal))
   ```

   - Calculates current collateralization ratio
   - Uses latest price feed

3. **Get Current Price**
   ```clarity
   (get-current-price)
   ```
   - Returns current BTC price
   - Used for calculations

#### Administrative Functions

1. **Set Price**

   ```clarity
   (set-price (new-price uint))
   ```

   - Updates BTC price
   - Restricted to price oracle

2. **Set Price Oracle**
   ```clarity
   (set-price-oracle (new-oracle principal))
   ```
   - Updates price oracle address
   - Restricted to contract owner

### Error Codes

| Code | Description             |
| ---- | ----------------------- |
| 1000 | Not authorized          |
| 1001 | Insufficient collateral |
| 1002 | Below minimum deposit   |
| 1003 | Invalid amount          |
| 1004 | Position not found      |
| 1005 | Already liquidated      |
| 1006 | Healthy position        |
| 1007 | Price expired           |

## Security Considerations

1. **Price Validity**

   - Price feeds expire after 144 blocks
   - Prevents stale price manipulation
   - Requires active oracle maintenance

2. **Collateralization Safety**

   - Conservative 150% minimum ratio
   - Early liquidation at 120%
   - Protects protocol solvency

3. **Flash Loan Protection**

   - Price validity windows
   - Atomic settlement
   - Position health checks

4. **Access Control**
   - Owner-only oracle updates
   - Oracle-only price updates
   - Protected administrative functions

## Integration Guide

### Initializing a Position

1. Call `deposit-collateral` with BTC amount
2. Verify position creation via `get-position`
3. Mint stablecoins within collateral limits
4. Monitor position health regularly

### Managing Risk

1. Maintain collateral ratio above 150%
2. Monitor BTC price movements
3. Repay debt or add collateral when near liquidation
4. Use `get-collateral-ratio` for position tracking

### Liquidation Prevention

1. Set up monitoring at 130% ratio
2. Prepare collateral top-up strategy
3. Maintain repayment reserves
4. Act before reaching 120% threshold
