# Audit-Temp

**Audit Firm:**
Jay audits
**Client Firm:**
Usdx
**Prepared By:**
Jay
**Delivery Date:**
12/11/23
<br />

**Protocol Name** 
Jay audits

# Project Overview

| Project Name | **Project Name**                 |
| ------------ | -------------------------------- |
| Language     | Solidity                         |
| Codebase     | (https://github.com/owenThurm/usdx-Slot0x0f-Jayfromthe13th)       |
| Commit       | [**\_**](https://github.com/___) |

| Delivery Date     | \_\_\_\_                        |
| ----------------- | ------------------------------- |
| Audit Methodology | Manual Review, Contract Fuzzing |

| Vulnerability Level   | Total | Pending | Declined | Acknowledged | Partially Resolved | Resolved |
| --------------------- | ----- | ------- | -------- | ------------ | ------------------ | -------- |
| [Critical](#Critical) | 2     | 0       | 0        | 0            | 0                  | 0        |
| [High](#High)         | 2     | 0       | 0        | 0            | 0                  | 0        |
| [Medium](#Medium)     | 6     | 0       | 0        | 0            | 0                  | 0        |
| [Low](#Low)           | 0     | 0       | 0        | 0            | 0                  | 0        |

# Audit Scope & Methodology

## Scope

| ID  | File                              | SHA-1 Hash                               |
| --- | --------------------------------- | ---------------------------------------- |
| DPT | contracts/DividendPayingToken.sol | 4bf3ea9b168bbd1bd61d8ae8583145b342b867ba |
| TOK | contracts/Token.sol               | 4bf3ea9b168bbd1bd61d8ae8583145b342b867ba |
| PRE | contracts/Presale.sol             | 4bf3ea9b168bbd1bd61d8ae8583145b342b867ba |
| AIR | contracts/Airdropper.sol          | 4bf3ea9b168bbd1bd61d8ae8583145b342b867ba |

## Methodology

The auditing process pays special attention to the following considerations:

- Testing the smart contracts against both common and uncommon attack vectors.
- Assessing the codebase to ensure compliance with current best practices and industry standards.
- Ensuring contract logic meets the specifications and intentions of the client.
- Cross-referencing contract structure and implementation against similar smart contracts produced by industry leaders.
- Thorough line-by-line manual review of the entire codebase by community auditors.

## Vulnerability Classifications

| Vulnerability Level   | Classification                                                                                                 |
| --------------------- | -------------------------------------------------------------------------------------------------------------- |
| [Critical](#Critical) | Easily exploitable by anyone, causing causing loss of assets or undermining of the protocol’s goals.           |
| [High](#High)         | Arduously exploitable by a subset of addresses, causing loss of assets or undermining of the protocol’s goals. |
| [Medium](#Medium)     | Inherent risk of future exploits that may or may not impact the smart contract execution.                      |
| [Low](#Low)           | Minor deviation from best practices.                                                                           |

# Invariants Assessed

During the course of the review, the following invariants were assesed and verified with X fuzzing runs:

- Invariant 1
- Invariant 2
- Invariant 3
- Invariant 4 (Optional)
- Invariant 5 (Optional)

# Findings & Resolutions

| ID           | Title                                                                                     | Category            | Severity | Status  |
| ------------ | ----------------------------------------------------------------------------------------- | ------------------- | -------- | ------- |
| [H-01](#H01) | A DOS can happen on XXXXXXX                                                               | DOS                 | HIGH     | Pending |
| [H-02](#H02) | Lack of access controls                                                                   | Access Controls     | HIGH     | Pending |
| [M-01](#M01) | Inherent risk of future exploits that may or may not impact the smart contract execution. | Token integration   | MEDIUM   | Pending |
| [M-02](#M02) | Minor deviation from best practices.                                                      | Logic error         | MEDIUM   | Pending |
| [L-01](#L01) | Low error 01                                                                              | Centralization risk | LOW      | Pending |
| [L-02](#L02) | Low error 02                                                                              | Validation          | LOW      | Pending |

## <a id="Critical"></a>Critical

### <a id="C01"></a> C-01 No Mechanism for Users to Retrieve Unstaked Tokens

https://github.com/owenThurm/usdx-Slot0x0f-Jayfromthe13th/blob/main/src/USDXGov.sol#L134

#### PoC:
https://github.com/owenThurm/usdx-Slot0x0f-Jayfromthe13th/blob/main/jay-audits%20/POCs/NO_MECHANISM_Unstaked_Token.t.sol
#### Description:

The identified vulnerability in the unstake function of the USDXGov.sol contract arises from the function's failure to actually transfer the unstaked tokens back to the user's wallet. While the function correctly updates the internal staked balances to reflect the unstaking action, it omits the crucial step of physically moving the tokens from the contract back to the user's account. This oversight results in a situation where, from the contract's perspective, the tokens appear to be unstaked, but in reality, they remain locked within the contract.

#### Recommendation:

Fixing the Unstake Function: The unstake function should be modified to include a transfer of the unstaked tokens back to the user. This can be achieved by calling a transfer function (e.g., ERC20.transfer) within unstake to move the tokens from the contract to the user's address.

## <a id="Critical"></a>Critical

### <a id="C02"></a> C-02 Unrestricted Access in manuallyUpdateInterest function

https://github.com/owenThurm/usdx-Slot0x0f-Jayfromthe13th/blob/main/src/USDX.sol#L340

#### PoC:
https://github.com/owenThurm/usdx-Slot0x0f-Jayfromthe13th/blob/main/jay-audits%20/POCs/Unrestricted_Access_manuallyUpdateInterest.sol
This test will effectively demonstrate the vulnerability by showing that any external user can manipulate the interest rate calculations of another user's loan. This unrestricted access can lead to system manipulation, increased transaction costs, potential system overload if misused for spamming, and unexpected financial outcomes.

#### Description:

The manuallyUpdateInterest function, accessible to any external account, raises concerns due to unrestricted access and absence of specific access controls. This could inadvertently allow users to update interest rates for others, leading to possible system manipulation. Also, the function's openness might lead to increased transaction costs and potential system overload if misused for spamming. Plus, the ease of altering interest calculations without constraints could result in unexpected financial outcomes and affect the balance of the system. There's also a risk of circumventing established thresholds or limits, leading to potential unexpected issues.

#### Recommendation:

To mitigate this issue, the manuallyUpdateInterest function should be modified to include access control. Only the user themself or an authorized entity (like a system administrator or a smart contract designed for this purpose) should be able to update the interest rates. This can be achieved by adding a check in the function to ensure that the caller is either the user in question or an authorized entity.

## <a id="High"></a> High

### <a id="H01"></a> H-01 No voting power for the staked tokens

https://github.com/owenThurm/usdx-Slot0x0f-Jayfromthe13th/blob/main/src/USDXGov.sol#L72-L73
https://github.com/owenThurm/usdx-Slot0x0f-Jayfromthe13th/blob/main/src/USDXGov.sol#L89

#### PoC:
https://github.com/owenThurm/usdx-Slot0x0f-Jayfromthe13th/blob/main/jay-audits%20/POCs/No_Voting_Power_Staked-H2.t.sol
#### Description:

In your contract, the voting power calculation, as it currently stands, does not take into account the tokens that users have staked. This limitation stems from the use of the getVotes function from the ERC20Votes contract, which is employed in critical functionalities like createProposal and vote. The getVotes function determines the voting power based on the user's current token balance, but crucially, it overlooks the staked balance. As a result, even when users stake their tokens, this action doesn’t reflect in their voting power. The voting power remains tied to the balance of unstaked tokens, disregarding any tokens that are locked up in staking.

#### Recommendation:

To align the voting power with both staked and unstaked tokens, a modification in the voting power calculation mechanism is required. This entails altering the standard behavior of the ERC20Votes contract. A feasible approach would be to revise the getVotes function, integrating the staked balance into the voting power computation. By doing so, the staked tokens would actively contribute to a user’s voting power, ensuring that the act of staking doesn’t diminish their influence in governance decisions. This change is crucial for ensuring that the voting power accurately reflects both the staked and unstaked token balances, aligning with the intended governance dynamics of the contract.


## <a id="High"></a> High

### <a id="H02"></a> H-02 Persistent Active Proposals due to Lack of Status Update Mechanism

[https://github.com/___/Contract.sol#L146-L160](https://github.com/owenThurm/usdx-Slot0x0f-Jayfromthe13th/blob/main/src/USDXGov.sol#L15-L22)
https://github.com/owenThurm/usdx-Slot0x0f-Jayfromthe13th/blob/main/src/USDXGov.sol#L103


#### PoC:
https://github.com/owenThurm/usdx-Slot0x0f-Jayfromthe13th/blob/main/jay-audits%20/POCs/Neverending_Proposal.t.sol

#### Description:

The identified vulnerability in the USDXGov contract arises from the lack of a proper status update mechanism for governance proposals. Specifically, the ProposalData struct and the executeProposal function are at the core of this issue.

Struct Limitation: The ProposalData struct defines properties of a proposal but crucially lacks a field to indicate its current status (e.g., Active, Executed, Failed).

Functionality Gap in executeProposal: The executeProposal function, intended to execute proposals, does not alter the proposal’s status upon execution. Consequently, a proposal, once created and marked as active, remains indefinitely in this state, irrespective of whether it has been executed or the voting period has lapsed.

This situation can lead to governance inefficiencies and confusion. Stakeholders might be unable to discern the current state of proposals, complicating decision-making and tracking of the governance process.





#### Recommendation:
Introduce a Status Field: Amend the ProposalData struct to include a status field. This could be an enum type with values like Active, Executed, Failed, etc. This change provides clarity on the proposal's lifecycle stage at any given moment.

Update executeProposal Function: Modify the executeProposal function to update the proposal's status at the end of its execution. The update could reflect whether the proposal met the execution criteria or if it expired without adequate votes. This modification ensures that each proposal’s state accurately reflects its real-world outcome.

Additional Safeguards: Implement checks in other functions interacting with proposals to respect the updated status. For instance, prevent voting on proposals marked as Executed or Failed.

## <a id="Medium"></a> Medium

### <a id="M01"></a> M-01 Insufficient Initial Liquidity Check

https://github.com/owenThurm/usdx-Slot0x0f-Jayfromthe13th/blob/main/src/USDX.sol#L160

#### Description:

In the initialLiquidity() function, it seems to be designed to mint an initial amount of liquidity for the contract, but it lacks a check to ensure that the amount of liquidity provided is sufficient. If the initial liquidity is insufficient, it could potentially unbalance the contract's operations.

The potential impact of this vulnerability is significant for the protocol. If the contract is expected to handle large amounts of liquidity, this vulnerability could be catastrophic. It could lead to unexpected behavior or even failure of certain operations. For instance, if the contract is designed to handle a certain amount of liquidity, and the initial liquidity is less than this amount, the contract might not function as expected.

Also, the lack of a check on the initial liquidity could potentially be exploited by malicious actors. For instance, if the contract owner were to mint a large amount of liquidity, they could potentially manipulate the contract's operations to their advantage.

#### Recommendation:

To address this issue, you should add a check for sufficient liquidity before minting the initial liquidity. This can be done by comparing the amount of liquidity provided with the expected amount of liquidity. If the provided amount is less than the expected amount, the transaction should be reverted. This ensures that the contract always has enough liquidity to function correctly.

## <a id="Medium"></a> Medium

### <a id="M02"></a> M-02 Inconsistent Voting Power Calculation in ERC20Votes

https://github.com/___/Contract.sol#L146-L160

#### Description:

In the current implementation of the contract, the voting power is determined based on the token balance at the time of voting, rather than considering the staked balance of the user. This means that if a user stakes their tokens, their voting power does not change immediately. The voting power is still based on the original balance of tokens, not the staked balance. This could potentially lead to issues if users are not aware of this requirement. For example, a user might stake their tokens and then find that they can't vote because their voting power hasn't updated yet.

Here's a step-by-step example of how a flash loan attack could be executed:

- The malicious actor observes a user staking a large amount of tokens.
- The malicious actor quickly borrows a large amount of tokens from a lending protocol using a flash loan.
- The malicious actor stakes the borrowed tokens, increasing their voting power.
- The malicious actor proposes a proposal.
- The malicious actor unstakes the borrowed tokens, returning them to the lending protocol and reducing their voting power.
- The malicious actor waits for the voting period to end, and then votes on the proposal to control its outcome.
- This attack is possible because the voting power is determined based on the token balance at the time of voting, not the staked balance of the user. If the staked balance of the user is taken into account when determining the voting power, this attack would not be possible.

#### Recommendation:

To remediate this issue, you should consider the staked balance of the user when determining their voting power. This can be done by modifying the createProposal and vote functions to take into account the staked balance of the user.

```solidity
function createProposal(address token, uint16 ltv) public {
   uint256 votingPower = getVotes(msg.sender);
   uint256 stakedBalance = userInfo[msg.sender].staked;
   require(votingPower + stakedBalance >= totalSupply() / 100, "Insufficient voting power to propose");

   // rest of the code...
}

function vote(uint256 proposalId) public {
   // rest of the code...

   uint256 votingPower = getPastVotes(msg.sender, proposal.createdAt);
   uint256 stakedBalance = userInfo[msg.sender].staked;
   require(votingPower + stakedBalance > 0, "No voting power");

   // rest of the code...
}
```

## <a id="Medium"></a> Medium

### <a id="M03"></a> M-03 Uncontrolled Increment of Reward Amount

https://github.com/owenThurm/usdx-Slot0x0f-Jayfromthe13th/blob/main/src/USDXGov.sol#L198

#### Description:

Description:
The vulnerability in the USDXGov.sol contract allows users to claim rewards without the corresponding fee accumulation in the system. This is caused by the repeated calling of the claimReward function without any fees being earned in the USDX contract. The virtualFees variable is not reset to zero after each call, leading to an inflated calculation of rewards.

Exploit Scenario:

- A malicious user repeatedly calls the claimReward function without any fees being earned in the USDX contract.
- The virtualFees variable is incremented with the totalFeesAvailable value each time the updateReward function is called.
- The user's reward is calculated based on the accumulated virtualFees, which is not the intended behavior.
- The user receives more rewards than they should, exploiting the vulnerability in the system.

#### Recommendation:

To fix this vulnerability, the virtualFees variable should be reset to zero at the end of the updateReward function. This will ensure that the calculation of rewards accurately reflects the actual fees earned and prevent the accumulation of virtualFees through repeated calls to claimReward without corresponding fee generation.

The following code snippet demonstrates the proposed fix:

```solidity
function updateReward(address account) internal {
    uint256 reward = earned(account);
    totalReward += reward;
    rewards[account] = totalReward;
    virtualFees = totalFeesAvailable; // Update virtualFees with the current total fees available
    if (reward > 0) {
        _mint(account, reward);
    }
    virtualFees = 0; // Reset virtualFees to zero after each update
}
```

By implementing this fix, the vulnerability will be closed, and users will only be able to claim rewards based on the actual fees earned in the USDX contract.

## <a id="Medium"></a> Medium

### <a id="M04"></a> M-04 Insufficient Balance Check on claimRewards function

[https://github.com/___/Contract.sol#L146-L160](https://github.com/owenThurm/usdx-Slot0x0f-Jayfromthe13th/blob/main/src/USDXGov.sol#L145)

#### Description:

The claimRewards function in the USDXGov.sol contract does not check the contract's balance before attempting to transfer rewards. This can lead to failed transactions and a loss of trust in the platform when users are expecting to receive rewards but the contract lacks the necessary tokens.

Exploit Scenario:

- Alice claims her rewards, and the contract has just enough tokens for her.
- The next person, Bob, tries to claim his rewards, but the contract runs out of tokens due to the lack of a balance check.
- Many users are unable to get their rewards, causing frustration and a loss of trust in the system.

#### Recommendation:

Remediation:
To fix this vulnerability, the claimReward function should include a check to ensure that the contract has enough of the reward token to cover the reward amount before attempting to transfer it. This can be done by comparing the contract's token balance with the reward amount and only proceeding with the transfer if the balance is sufficient. If the balance is insufficient, the function should revert or handle the situation gracefully, potentially queuing the reward for future distribution.

The following code snippet demonstrates the proposed fix:

```solidity
function claimReward() external {
    uint256 reward = rewards[msg.sender];
    require(reward > 0, "No rewards to claim");

    uint256 contractBalance = ERC20(rewardToken).balanceOf(address(this));
    require(contractBalance >= reward, "Insufficient contract balance");

    rewards[msg.sender] = 0;
    ERC20(rewardToken).transfer(msg.sender, reward);
    emit RewardClaimed(msg.sender, reward);
}
```

By implementing this fix, the claimRewards function will check the contract's balance before attempting to transfer rewards, preventing failed transactions and ensuring a more reliable and trustworthy platform for users.

## <a id="Medium"></a> Medium

### <a id="M05"></a> M-05 Insufficient Balance Check in Staking Function

https://github.com/owenThurm/usdx-Slot0x0f-Jayfromthe13th/blob/main/src/USDXGov.sol#L122

#### Description:

The USDXGov.sol contract allows users to stake tokens without checking their balance, which can lead to potential issues such as token distribution imbalance and draining the contract of tokens. A malicious user could stake more tokens than they own, causing a discrepancy in the staked field of the User struct and the user's actual balance. When the user tries to unstake their tokens, the contract would revert due to insufficient balance, locking the user's tokens in the contract indefinitely.

#### Recommendation:

Remediation:
Implement balance checks for staking and unstaking tokens.
Add a mechanism to mark proposals as completed or unsuccessful.

By addressing these issues, the contract will be able to function as intended and ensure that users can interact with it in a safe and secure manner.

## <a id="Medium"></a> Medium

### <a id="M06"></a> M-06 Incomplete Implementation of Fees Distribution in \_updateInterest Function

https://github.com/owenThurm/usdx-Slot0x0f-Jayfromthe13th/blob/main/src/USDX.sol#L356

#### Description:

The \_updateInterest function in the contract contains a TODO comment indicating that the excessBucket variable should be updated. This variable is crucial for the correct calculation of fees earned by the contract. If this line of code is not implemented, it could lead to incorrect calculations when the contract needs to distribute fees to the governance (\_gov) account. The withdrawFees function, for example, transfers the value of excessBucket to the \_gov account. If excessBucket is not updated correctly, the \_gov account may not receive the correct amount of fees. This could lead to significant financial loss for the governance account. Furthermore, it could also affect the overall functionality of the contract, as the contract relies on accurate fee calculations for its operations.

#### Recommendation:

In this function, the contract transfers the value of feesEarned to the \_gov account, then sets feesEarned to 0. If the line of code to update excessBucket is not implemented, feesEarned may not be set to 0 correctly, which could lead to incorrect calculations in the withdrawFees function.


