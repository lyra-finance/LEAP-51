---
leap: 49
title: Fee Referral Program
status: Proposed
author: dappbeast
created: 2023-2-9
---

## Simple Summary

A fee referral program that rewards traders, apps and integrations that drive volume through the Lyra Protocol.

## Abstract

This LEAP proposes a fee referral program as an extension of the trading rewards program outlined in [LEAP 34](https://leaps.lyra.finance/leaps/leap-34/) that rewards traders, apps and integrations that drive volume through the Lyra Protocol. It also proposes some simplifications to the current trading rewards program for better trading and staking UX.

## Motivation

While the current trading rewards program has successfully retained existing traders and aligned them with the DAO, it has broadly failed to attract new traders and new sources of volume. A simple referral program that rewards individual referrers, apps such as Kwenta, and integrations such as Polynomial, will help create a referral flywheel and generate more protocol volumes.

Additionally, the current fee tiers [dune query](https://dune.com/queries/1926613) of active traders on Optimism shows high drop off after the 1m stkLYRA fee tier.

## Specification

### Referral Tiers

The fee referral program is split into two tiers for referrers:

- Tier 1: Extra 5% fee rebate to traders, 5% fee reward to referrer.
- Tier 2: Extra 10% fee rebate to traders, 10% fee reward to referrer.

All referrers automatically qualify for Tier 1. To qualify for Tier 2:

- Referrers must be manually allowlisted by the Council in public forums and hardcoded in open source repos calculating trading rewards. This incentivises referrers attracting substantial volumes to be transparent with the community and disincentivises malicious activities like wash trading.
- Referrers must generate $10,000 in fees over the 2 week epoch. This incentivises successful referrers to maintain Tier 2 status.

Additional properties of the referral program:
- Rebates / rewards are distributed as stkLYRA in 2 week epochs, with reward caps set by Council.
- Referrers will only be eligible on Newport deployments which include the `referrer` parameter in `OptionMarket`. This means the fee referral program will only be live on Arbitrum initially, but the proposed changes to fee tiers will apply to both Arbitrum and Optimism traders to simplify programs and keep them consistent.

### Fee Tiers

Fee tiers have been adjusted to account for the 5%/10% bonus in fee rebates rewarded to traders. Tiers have also been merged or removed based on analysis of existing traders and stkLYRA holders on Optimism [here](https://dune.com/queries/1926613).

**Old Program**
| stkLYRA Balance | Rebate |
| ------------- | ------------- |
| 0 | 5% | 
| 1,000 | 20% | 
| 5,000 | 30% | 
| 10,000 | 35% | 
| 20,000 | 40% |
| 50,000 | 45% | 
| 100,000 | 47.5% | 
| 250,000 | 50% | 
| 500,000 | 52.5% | 
| 1,000,000 | 55% | 
| 2,000,000 | 57.5% | 
| 3,000,000 | 60% |

**New Program (No Referral Bonus\*)**
| stkLYRA Balance | Rebate |
| ------------- | ------------- |
| 1,000 | 10% | 
| 5,000 | 20% | 
| 10,000 | 30% | 
| 50,000 | 40% | 
| 250,000 | 50% | 
| 1,000,000 | 60% | 

\* With a Tier 2 Referrer (which should be the default for most traders, see "Examples" below), all tiers receive an additional 10% rebate, increasing the maximum rebate to 70%.

### Examples

The fee referral program can be utilized by developers in many ways:

- An application such as Kwenta can host their own interface to the Lyra Protocol and hardcode their referrer address in the frontend. They will receive rewards proportional to how many traders use their interface.
	- An application following this model could run a "nested" referral program for their traders. E.g. Kwenta DAO could distribute a portion of their referral rewards to traders by running their own referral program on top of the Lyra DAO's.
- An integration such as Polynomial can hardcode their referrer address into on-chain calls to Newport contracts. They will receive rewards proportional to volume generated by their vaults.
- An agnostic application could allow traders to input their own referral codes, rewarding individual referrers directly.

### Implementation

Referrals will be tracked via the `referrer` parameter in `OptionMarket` trade functions. The volume generated by each referral address will be aggregated in an off-chain trading rewards script and distributed in 2 week epochs. Rewards will be claimable by the referrer via the distributor contract used for other epoch rewards.

## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).