---
layout: twoColumn
section: Customer verification
type: article
title:  "Handling verification statuses"
weight: 2
description: "How to verify a customer before sending a bank transfer with Dwolla's ACH API."
---

# Customer verification

## Handling verification statuses

There are various reasons a Customer will result in a status other than `verified` which you will want to account for after the Customer is created. For example, the `retry` status can occur when an individual mis-keys or uses incorrect identifying information upon Customer creation. 

#### Testing verification statuses in Sandbox: 
By submitting `verified`, `retry`, `document`, or `suspended` in the firstName parameter, you can create a new verified Customer with that status. To get a verified Customer with the `retry` status to `verified`, you need to POST to /customers/{id} with `verified` or anything else and the verified Customer will get `verified`.  The full SSN is required in the when updating a Customer with a `retry` verification status.  If only the last four of the SSN is submitted, Dwolla returns "invalid SSN", and initiating a POST again with a Full SSN will result in a `document` status.

If all required information has previosly been submitted, a customer can be moved from `retry` to `verified` in the sandbox.

```raw
POST https://api.dwolla.com/customers/132681fa-1b4d-4181-8ff2-619ca46235b1
Content-Type: application/vnd.dwolla.v1.hal+json
Accept: application/vnd.dwolla.v1.hal+json
Authorization: Bearer pBA9fVDBEyYZCEsLf/wKehyh1RTpzjUj5KzIRfDi0wKTii7DqY

{
  "firstName": "verified",
  "lastName": "Doe",
  "email": "johndoe@nomail.net",
  "type": "personal",
  "ssn ": "202-99-1516"
}
```
```ruby
customer_url = 'https://api.dwolla.com/customers/132681fa-1b4d-4181-8ff2-619ca46235b1'
request_body = {
      "firstName" => "verified",
       "lastName" => "Doe",
          "email" => "jdoe@nomail.com",
           "type" => "personal",
            "ssn" => "123-45-6789"
}

# Using DwollaV2 - https://github.com/Dwolla/dwolla-v2-ruby (Recommended)
customer = account_token.post customer_url, request_body
customer.id # => "132681fa-1b4d-4181-8ff2-619ca46235b1"

# Using DwollaSwagger - https://github.com/Dwolla/dwolla-swagger-ruby
customer = DwollaSwagger::CustomersApi.update_customer(customer_url, :body => request_body)
customer.id # => "132681fa-1b4d-4181-8ff2-619ca46235b1"
```
```javascript
// Using dwolla-v2 - https://github.com/Dwolla/dwolla-v2-node
var customerUrl = 'https://api.dwolla.com/customers/132681fa-1b4d-4181-8ff2-619ca46235b1';
var requestBody = {
  firstName: "verified",
  lastName: "Doe",
  email: "johndoe@dwolla.com",
  type: "personal",
  ssn: "202-99-1516"
};

accountToken
  .post(customerUrl, requestBody)
  .then(function(res) {
    res.body.id; // => '132681fa-1b4d-4181-8ff2-619ca46235b1'
  });
```
```python
customer_url = 'https://api.dwolla.com/customers/132681fa-1b4d-4181-8ff2-619ca46235b1'
request_body = {
  "firstName": "verified",
  "lastName": "Doe",
  "email": "jdoe@nomail.com",
  "type": "personal",
  "ssn": "123-45-6789"
}

# Using dwollav2 - https://github.com/Dwolla/dwolla-v2-python (Recommended)
customer = account_token.post('customers', request_body)
customer.body.id # => '132681fa-1b4d-4181-8ff2-619ca46235b1'

# Using dwollaswagger - https://github.com/Dwolla/dwolla-swagger-python
customers_api = dwollaswagger.CustomersApi(client)
customer = customers_api.update_customer(customer_url, body = request_body)
customer.id # => '132681fa-1b4d-4181-8ff2-619ca46235b1'
```
```php
<?php
$customersApi = DwollaSwagger\CustomersApi($apiClient);

$retryCustomer = $customersApi->create(array (
  'firstName' => 'verified',
  'lastName' => 'Doe',
  'email' => 'johndoe@nomail.net',
  'type' => 'personal',
  'ssn' => '202-99-1516',
));

print($retryCustomer); # => 132681fa-1b4d-4181-8ff2-619ca46235b1
?>
```

### Handling status: `retry`

If the Customer has a status of `retry`, some information may have been miskeyed. You have one more opportunity to correct any mistakes. This time, you’ll need to provide the customer’s full SSN.

```raw
POST https://api.dwolla.com/customers/132681fa-1b4d-4181-8ff2-619ca46235b1
Content-Type: application/vnd.dwolla.v1.hal+json
Accept: application/vnd.dwolla.v1.hal+json
Authorization: Bearer pBA9fVDBEyYZCEsLf/wKehyh1RTpzjUj5KzIRfDi0wKTii7DqY

{
  "firstName": "John",
  "lastName": "Doe",
  "email": "johndoe@nomail.net",
  "ipAddress": "10.10.10.10",
  "type": "personal",
  "address1": "221 Corrected Address St.",
  "address2": "Fl 8",
  "city": "Ridgewood",
  "state": "NY",
  "postalCode": "11385",
  "dateOfBirth": "1990-07-11",
  "ssn ": "202-99-1516"
}
```
```ruby
customer_url = 'https://api.dwolla.com/customers/132681fa-1b4d-4181-8ff2-619ca46235b1'
request_body = {
      "firstName" => "John",
       "lastName" => "Doe",
          "email" => "jdoe@nomail.com",
      "ipAddress" => "10.10.10.10",
           "type" => "personal",
       "address1" => "221 Corrected Address St..",
       "address2" => "Apt 201",
           "city" => "San Francisco",
          "state" => "CA",
     "postalCode" => "94104",
    "dateOfBirth" => "1970-07-11",
            "ssn" => "123-45-6789"
}

# Using DwollaV2 - https://github.com/Dwolla/dwolla-v2-ruby (Recommended)
customer = account_token.post customer_url, request_body
customer.id # => "132681fa-1b4d-4181-8ff2-619ca46235b1"

# Using DwollaSwagger - https://github.com/Dwolla/dwolla-swagger-ruby
customer = DwollaSwagger::CustomersApi.update_customer(customer_url, :body => request_body)
customer.id # => "132681fa-1b4d-4181-8ff2-619ca46235b1"
```
```javascript
// Using dwolla-v2 - https://github.com/Dwolla/dwolla-v2-node
var customerUrl = 'https://api.dwolla.com/customers/132681fa-1b4d-4181-8ff2-619ca46235b1';
var requestBody = {
  firstName: "John",
  lastName: "Doe",
  email: "johndoe@dwolla.com",
  ipAddress: "10.10.10.10",
  type: "personal",
  address1: "221 Corrected Address St..",
  address2: "Fl 8",
  city: "Ridgewood",
  state: "NY",
  postalCode: "11385",
  dateOfBirth: "1990-07-11",
  ssn: "202-99-1516"
};

accountToken
  .post(customerUrl, requestBody)
  .then(function(res) {
    res.body.id; // => '132681fa-1b4d-4181-8ff2-619ca46235b1'
  });
```
```python
customer_url = 'https://api.dwolla.com/customers/132681fa-1b4d-4181-8ff2-619ca46235b1'
request_body = {
  "firstName": "John",
  "lastName": "Doe",
  "email": "jdoe@nomail.com",
  "ipAddress": "10.10.10.10",
  "type": "personal",
  "address1": "221 Corrected Address St..",
  "address2": "Apt 201",
  "city": "San Francisco",
  "state": "CA",
  "postalCode": "94104",
  "dateOfBirth": "1970-07-11",
  "ssn": "123-45-6789"
}

# Using dwollav2 - https://github.com/Dwolla/dwolla-v2-python (Recommended)
customer = account_token.post('customers', request_body)
customer.body.id # => '132681fa-1b4d-4181-8ff2-619ca46235b1'

# Using dwollaswagger - https://github.com/Dwolla/dwolla-swagger-python
customers_api = dwollaswagger.CustomersApi(client)
customer = customers_api.update_customer(customer_url, body = request_body)
customer.id # => '132681fa-1b4d-4181-8ff2-619ca46235b1'
```
```php
<?php
$customersApi = DwollaSwagger\CustomersApi($apiClient);

$retryCustomer = $customersApi->create(array (
  'firstName' => 'John',
  'lastName' => 'Doe',
  'email' => 'johndoe@nomail.net',
  'ipAddress' => '10.10.10.10',
  'type' => 'personal',
  'address1' => '221 Corrected Address St.',
  'address2' => 'Fl 8',
  'city' => 'Ridgewood',
  'state' => 'NY',
  'postalCode' => '11385',
  'dateOfBirth' => '1990-07-11',
  'ssn' => '202-99-1516',
));

print($retryCustomer); # => 132681fa-1b4d-4181-8ff2-619ca46235b1
?>
```

Check the Customer’s status again. The Customer will either be `verified` or in the `document` or `suspended` state of verification.

### Handling status: `document`

If the Customer has a status of `document`, then you'll need to upload additional pieces of information in order to verify the account. Use the [create a document](https://docsv2.dwolla.com/#create-a-document) endpoint when uploading a scan of the identifying document. The document will then be reviewed by Dwolla.

#### Document Types
**Personal verified Customers:** a scanned photo of the Customer's identifying document can be specified as documentType: `passport`, `license` (state issued driver's license), or `idCard` (other U.S. government-issued photo id card).

**Business verified Customers:** Documents that are used to help identify a business are specified as documentType `other`. Business Identifying documents can include the following:

* Partnership, General Partnership: EIN Letter (IRS-issued SS4 confirmation letter).
* Limited Liability Corporation (LLC), Corporation: EIN Letter (IRS-issued SS4 confirmation letter).
* Sole Proprietorship: one or more of the following, as applicable to your sole proprietorship: Fictitious Business Name Statement; EIN documentation (IRS-issued SS4 confirmation letter); Color copy of a valid government-issued photo ID (e.g., a driver’s license, passport, or state ID card).

```raw
curl -X POST 
\ -H "Authorization: Bearer tJlyMNW6e3QVbzHjeJ9JvAPsRglFjwnba4NdfCzsYJm7XbckcR" 
\ -H "Accept: application/vnd.dwolla.v1.hal+json" 
\ -H "Cache-Control: no-cache" 
\ -H "Content-Type: multipart/form-data" 
\ -F "documentType=passport" 
\ -F "file=@foo.png" 
\ 'https://api-uat.dwolla.com/customers/132681fa-1b4d-4181-8ff2-619ca46235b1/documents'

HTTP/1.1 201 Created
Location: https://api-uat.dwolla.com/documents/11fe0bab-39bd-42ee-bb39-275afcc050d0
```
```ruby
# Using DwollaV2 - https://github.com/Dwolla/dwolla-v2-ruby
customer_url = 'https://api.dwolla.com/customers/132681fa-1b4d-4181-8ff2-619ca46235b1'

file = Faraday::UploadIO.new('mclovin.jpg', 'image/jpeg')
document = account_token.post "#{customer_url}/documents", file: file, documentType: 'license'
document.headers[:location] # => "https://api.dwolla.com/documents/11fe0bab-39bd-42ee-bb39-275afcc050d0"
```
```javascript
// Using dwolla-v2 - https://github.com/Dwolla/dwolla-v2-node
var customerUrl = 'https://api.dwolla.com/customers/132681fa-1b4d-4181-8ff2-619ca46235b1';

var requestBody = new FormData();
body.append('file', fs.createReadStream('mclovin.jpg'), {
  filename: 'mclovin.jpg',
  contentType: 'image/jpeg',
  knownLength: fs.statSync('mclovin.jpg').size
});
body.append('documentType', 'license');

accountToken
  .post(`${customerUrl}/documents`, requestBody)
  .then(function(res) {
    res.headers.get('location'); // => "https://api.dwolla.com/documents/11fe0bab-39bd-42ee-bb39-275afcc050d0"
  });
```
```python
customer_url = 'https://api.dwolla.com/customers/132681fa-1b4d-4181-8ff2-619ca46235b1'

# Using dwollav2 - https://github.com/Dwolla/dwolla-v2-python (Recommended)
document = account_token.post('%s/documents' % customer_url, file = open('mclovin.jpg', 'rb'), documentType = 'license')
document.headers['location'] # => 'https://api.dwolla.com/documents/11fe0bab-39bd-42ee-bb39-275afcc050d0'
```
```php
// No SDK support. Coming soon
```

If the document was successfully uploaded, the response will be a HTTP 201|Created with the URL of the new document resource contained in the Location header.

```noselect
HTTP/1.1 201 Created
Location: https://api-uat.dwolla.com/documents/11fe0bab-39bd-42ee-bb39-275afcc050d0
```

You’ll also get a webhook with a `customer_verification_document_uploaded` event to let you know the document was successfully uploaded.

Once created, the document will be reviewed by Dwolla. When our team has made a decision, we’ll trigger either a `customer_verification_document_approved` or `customer_verification_document_failed`event (possibly followed by another `customer_verification_document_needed`).

If the document was sufficient, the Customer will be verified. If not, we may need additional documentation.

If the document was found to be fraudulent or doesn’t match the identity of the Customer, they will be suspended.

#### Document failure
If you receive a `customer_verification_document_failed` webhook, you’ll need to upload another document. The document can fail if, for example, the Customer uploaded the wrong type of document or the `.jpg` or `.png` file supplied was not readable (i.e. blurry, not well lit, or cuts off a portion of the identifying image). To retrieve the failure reason for the document upload, you’ll retrieve the document by its id. Contained in the response will be a `failureReason` which corresponds to one of the following values:

* ScanNotReadable
* ScanNotUploaded
* ScanIdTypeNotSupported
* ScanNameMismatch
* ScanFailedOther
* FailedOther

#### Request and response:

```raw
GET https://api-uat.dwolla.com/documents/11fe0bab-39bd-42ee-bb39-275afcc050d0
Accept: application/vnd.dwolla.v1.hal+json
Authorization: Bearer tJlyMNW6e3QVbzHjeJ9JvAPsRglFjwnba4NdfCzsYJm7XbckcR

...

{
  "_links": {
    "self": {
      "href": "https://api-uat.dwolla.com/documents/11fe0bab-39bd-42ee-bb39-275afcc050d0"
    }
  },
  "id": "11fe0bab-39bd-42ee-bb39-275afcc050d0",
  "status": "reviewed",
  "type": "license",
  "created": "2016-01-29T21:22:22.000Z",
  "failureReason": "ScanNotReadable"
}
```
```ruby
document_url = 'https://api.dwolla.com/documents/11fe0bab-39bd-42ee-bb39-275afcc050d0'

# Using DwollaV2 - https://github.com/Dwolla/dwolla-v2-ruby (Recommended)
document = account_token.get document_url
document.failureReason # => "ScanNotReadable"

# Using DwollaSwagger - https://github.com/Dwolla/dwolla-swagger-ruby
document = DwollaSwagger::DocumentsApi.get_document(document_url)
document.failureReason # => "ScanNotReadable"
```
```javascript
var documentUrl = 'https://api.dwolla.com/documents/11fe0bab-39bd-42ee-bb39-275afcc050d0';

accountToken
  .get(document_url)
  .then(function(res) {
    res.body.failureReason; // => "ScanNotReadable"
  });
```
```python
document_url = 'https://api.dwolla.com/documents/11fe0bab-39bd-42ee-bb39-275afcc050d0'

# Using dwollav2 - https://github.com/Dwolla/dwolla-v2-python (Recommended)
documents = account_token.get(document_url)
documents.body['failureReason'] # => 'ScanNotReadable'

# Using dwollaswagger - https://github.com/Dwolla/dwolla-swagger-python
documents_api = dwollaswagger.DocumentsApi(client)
document = documents_api.get_customer(document_url)
document.failureReason # => "ScanNotReadable"
```
```php
<?php
$aDocument = 'https://api.dwolla.com/documents/11fe0bab-39bd-42ee-bb39-275afcc050d0';

$documentsApi = DwollaSwagger\DocumentsApi($apiClient);

$retrieved = $documentsApi->getCustomer($aDocument);
print($retrieved->failureReason); # => "ScanNotReadable"
?>
```

### Handling status: `suspended`

If the Customer is `suspended`, there’s no further action you can take to correct this using the API. You’ll need to contact support@dwolla.com for assistance.
