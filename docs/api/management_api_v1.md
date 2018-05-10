Management API v1
=================
Management API is server-to-server API with high privileges.

**Version:** 1.7.12

**License:** https://github.com/rubykube/peatio/blob/master/LICENSE.md

### /v1/deposits
---
##### ***POST***
**Summary:** Returns deposits as paginated collection.

**Description:** Returns deposits as paginated collection.

**Parameters**

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ---- |
| uid | formData | The shared user ID. | No | string |
| currency | formData | The currency code. | No | string |
| page | formData | The page number (defaults to 1). | No | integer |
| limit | formData | The number of deposits per page (defaults to 100, maximum is 1000). | No | integer |
| state | formData | The state to filter by. | No | string |

**Responses**

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 201 | Returns deposits as paginated collection. | [Deposit](#deposit) |

### /v1/deposits/get
---
##### ***POST***
**Summary:** Returns deposit by TID.

**Description:** Returns deposit by TID.

**Parameters**

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ---- |
| tid | formData | The transaction ID. | Yes | string |

**Responses**

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 201 | Returns deposit by TID. | [Deposit](#deposit) |

### /v1/deposits/new
---
##### ***POST***
**Summary:** Creates new fiat deposit with state set to «submitted». Optionally pass field «state» set to «accepted» if want to load money instantly. You can also use PUT /fiat_deposits/:id later to load money or cancel deposit.

**Description:** Creates new fiat deposit with state set to «submitted». Optionally pass field «state» set to «accepted» if want to load money instantly. You can also use PUT /fiat_deposits/:id later to load money or cancel deposit.

**Parameters**

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ---- |
| uid | formData | The shared user ID. | Yes | string |
| tid | formData | The shared transaction ID. Must not exceed 64 characters. Peatio will generate one automatically unless supplied. | No | string |
| currency | formData | The currency code. | Yes | string |
| amount | formData | The deposit amount. | Yes | double |
| state | formData | The state of deposit. | No | string |

**Responses**

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 201 | Creates new fiat deposit with state set to «submitted». Optionally pass field «state» set to «accepted» if want to load money instantly. You can also use PUT /fiat_deposits/:id later to load money or cancel deposit. | [Deposit](#deposit) |

### /v1/deposits/state
---
##### ***PUT***
**Summary:** Allows to load money or cancel deposit.

**Description:** Allows to load money or cancel deposit.

**Parameters**

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ---- |
| tid | formData | The shared transaction ID. | Yes | string |
| state | formData | The new state to apply. | Yes | string |

**Responses**

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | Allows to load money or cancel deposit. | [Deposit](#deposit) |

### /v1/withdraws
---
##### ***POST***
**Summary:** Returns withdraws as paginated collection.

**Description:** Returns withdraws as paginated collection.

**Parameters**

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ---- |
| uid | formData | The shared user ID. | No | string |
| currency | formData | The currency code. | No | string |
| page | formData | The page number (defaults to 1). | No | integer |
| limit | formData | The number of objects per page (defaults to 100, maximum is 1000). | No | integer |
| state | formData | The state to filter by. | No | string |

**Responses**

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 201 | Returns withdraws as paginated collection. | [Withdraw](#withdraw) |

### /v1/withdraws/get
---
##### ***POST***
**Summary:** Returns withdraw by ID.

**Description:** Returns withdraw by ID.

**Parameters**

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ---- |
| tid | formData | The shared transaction ID. | Yes | string |

**Responses**

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 201 | Returns withdraw by ID. | [Withdraw](#withdraw) |

### /v1/withdraws/new
---
##### ***POST***
**Summary:** Creates new withdraw.

**Description:** You can pass «state» set to «submitted» if you want to start processing withdraw.

**Parameters**

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ---- |
| uid | formData | The shared user ID. | Yes | string |
| tid | formData | The shared transaction ID. Must not exceed 64 characters. Peatio will generate one automatically unless supplied. | No | string |
| rid | formData | The beneficiary ID or wallet address on the Blockchain. | Yes | string |
| currency | formData | The currency code. | Yes | string |
| amount | formData | The amount to withdraw. | Yes | double |
| state | formData | The withdraw state to apply. | No | string |

**Responses**

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 201 | Creates new withdraw. | [Withdraw](#withdraw) |

### /v1/withdraws/state
---
##### ***PUT***
**Summary:** Updates withdraw state.

**Description:** «submitted» – system will check for suspected activity, lock the money, and process the withdraw. «canceled» – system will mark withdraw as «canceled», and unlock the money.

**Parameters**

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ---- |
| tid | formData | The shared transaction ID. | Yes | string |
| state | formData |  | Yes | string |

**Responses**

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | Updates withdraw state. | [Withdraw](#withdraw) |

### /v1/timestamp
---
##### ***POST***
**Summary:** Returns server time in seconds since Unix epoch.

**Description:** Returns server time in seconds since Unix epoch.

**Responses**

| Code | Description |
| ---- | ----------- |
| 201 | Returns server time in seconds since Unix epoch. |

### Models
---

### Deposit  

| Name | Type | Description | Required |
| ---- | ---- | ----------- | -------- |
| tid | integer | The shared transaction ID. | No |
| currency | string | The currency code. | No |
| uid | string | The shared user ID. | No |
| type | string | The deposit type (fiat or coin). | No |
| amount | string | The deposit amount. | No |
| state | string | The deposit state. «submitted» – initial state. «canceled» – deposit has been canceled by outer service. «rejected» – deposit has been rejected by outer service.. «accepted» – deposit has been accepted by outer service, money are loaded. | No |
| created_at | string | The datetime when deposit was created. | No |
| completed_at | string | The datetime when deposit was completed. | No |
| blockchain_txid | string | The transaction ID on the Blockchain (coin only). | No |
| blockchain_confirmations | string | The number of transaction confirmations on the Blockchain (coin only). | No |

### Withdraw  

| Name | Type | Description | Required |
| ---- | ---- | ----------- | -------- |
| tid | integer | The shared transaction ID. | No |
| uid | string | The shared user ID. | No |
| currency | string | The currency code. | No |
| type | string | The withdraw type (fiat or coin). | No |
| amount | string | The withdraw amount excluding fee. | No |
| fee | string | The exchange fee. | No |
| rid | string | The beneficiary ID or wallet address on the Blockchain. | No |
| state | string | The withdraw state. «prepared» – initial state, money are not locked. «submitted» – withdraw has been allowed by outer service for further validation, money are locked. «canceled» – withdraw has been canceled by outer service, money are unlocked. «accepted» – system has validated withdraw and queued it for processing by worker, money are locked. «rejected» – system has validated withdraw and found errors, money are unlocked. «suspected» – system detected suspicious activity, money are unlocked. «processing» – worker is processing withdraw as the current moment, money are locked. «succeed» – worker has successfully processed withdraw, money are subtracted from the account. «failed» – worker has encountered an unhandled error while processing withdraw, money are unlocked. | No |
| created_at | string | The datetime when withdraw was created. | No |
| blockchain_txid | string | The transaction ID on the Blockchain (coin only). | No |