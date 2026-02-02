# [Artist Dashboard] Ability for User to Purchase Contracts and Licenses

| Field | Value |
|-------|-------|
| **Document status** | Ready for review |
| **Document owner** | @Yuliya Kuzmina |
| **Approved by** | — |
| **Approval date** | — |
| **Jira ticket** | DWP-12079: [Artist Dashboard] Ability for User to purchase Contracts and Licenses (In Progress) |

---

## Goals

1. To provide users with an ability to purchase contracts and licenses via Ditto system, so that to simplify collaborations and protect their rights.

---

## Prototype

The source of truth is the Requirements section below. Design can slightly differ.

**Link to Figma wireframes:** Ditto Music Master Figma

---

## Assumptions and Constraints

1. Approval step will be skipped in Contract lifecycle: [Juro API documentation](https://docs.juro.com)
2. The contracts will be sent for signing to the signatories automatically once the draft contract is created on Juro side.
3. Basic info about contracts management in Juro: [Creating, approving, reviewing and signing contracts in Juro | Juro Help Center](https://help.juro.com)

---

## Requirements

1. When user selects a contract in **Contracts section** of View artist page, the system should open Contract details form (see details below).
   - *Design to be provided: DC-116: [Design Needed][Web] Juro contract - a form with the fields required for the contract (To Do)*

2. In Contract details form user can populate the required info and submit contract request.

3. When the request is submitted, the system should:
   1. Create a draft contract.
   2. Add the contract to **the list of user contracts** with the corresponding status (see below).
   3. Add the contract to the basket.
   4. Navigate user to Basket page.

4. User can add multiple contracts to the basket and proceed as one order.

5. When the order is paid, the system should:
   1. Send contract details to Juro for generating the contract.
   2. Update contract status to 'Processing'.
   3. Show Order confirmation screen with a "Back to Contracts" button, should navigate user back to Contracts page. *(Phase 2)*

6. If user adds a contract to basket but doesn't complete payment, the system should remove it from basket when the session ends/expires - same logic as for MR.

7. User can Edit contracts in 'Draft' status. The system should open Contract details form in Edit mode. When updated, the system should show the toast message: **"Contract details are successfully updated"**

8. User can Delete contracts in 'Draft' and "Removed" status. When activated, the system should delete the contract and show the toast message: **"The contract is deleted"**

---

## User Interface and Data Requirements

### Contract Statuses

| Status | Description |
|--------|-------------|
| **Draft** | Displayed by default, unless contract is generated on Juro side. |
| **Processing** | Shown once contract is generated on Juro side but not yet approved and signed. |
| **Ready** | Shown once the contract is finalized (approved and signed) and sent to user email. |
| **Ended** | Shown once the end date of the contract comes. |
| **Removed** | Shown once contract is deleted on Juro side. |

### Contract Details - Exclusive Recording Agreement

Should include the following fields:

| Field | Type | Prepopulated? |
|-------|------|---------------|
| Record Label Name | Text input | NO |
| Record Label Address | Text input | NO |
| Record Label City, Country, Postcode | Text input | NO |
| Record Label Email | Text input | NO |
| Record Label Phone | Text input | NO |
| Date | Text input | YES, current date |
| Artist/Band Name | Text input | YES |
| Artist/Band Address | Text input | NO |
| Artist/Band City, Country, Postcode | Text input | NO |
| Effective date | Text input | YES, current date |
| Term | Text input | NO |
| Service Start Date | Text input | YES, current date |
| Service End Date | Text input | NO |
| Number of full-length albums | Text input | NO |
| Payment Structure | Text input | NO |
| Cure Period Section | Text input | NO |
| Governing Law | Text input | NO |

#### Record Label Signature

| Field | Type | Prepopulated? |
|-------|------|---------------|
| Signature | eSign area | NO |
| Signatory name | Text input | NO |
| Title (Record Label) | Text input | NO |
| Email of signatory | Text input | NO |
| Timestamp | Text input | NO |

#### Artist/Band Signature

| Field | Type | Prepopulated? |
|-------|------|---------------|
| Signature | eSign area | NO |
| Signatory name | Text input | YES |
| Title (Artist/Band) | Text input | YES, Artist name |
| Email of signatory | Text input | NO |
| Timestamp | Text input | YES, current time |

### Actions

| Action | Type |
|--------|------|
| Submit | Button |
| Cancel | Button |

---

## Discussion Points

N/A
