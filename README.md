# ASEANZK-CP4I-Practicum-apic
CP4I API Connect enablement 
Tutorial Overview
<img width="698" alt="image" src="https://user-images.githubusercontent.com/25983259/193075472-7df79cae-05e1-49bb-946c-258aaab1e55f.png">

Turotial Use Case:

  Bank needs to provide customer account information to digital channels. e.g Fintech

  Create an API to allow digital channel developer to access & test the API from Bank’s developer portal.

  Customer account information is stored in core banking system with the virtual API endpoint: 
(https://cp4i-mock-api.mybluemix.net/accounts)

  Sensitive data such as Identity Card No. & Phone No. must be masked before sending response back to the consumer app.

  Define API Plan with the quota,  e.g. 100 calls per minute

  Only the authorized consumer app can consume the API with valid Client ID key.

Tutorial Context:

<img width="606" alt="image" src="https://user-images.githubusercontent.com/25983259/193087744-8569dce7-bb37-4be6-9d8a-1be8264dc287.png">


Topic 1: Bank developer persona - Create API for external channel to access

Topic 2: Bank admin persona -  Define API Plan and Publish API

Topic 3: Consumer organisation developer persona - Self Service on-boarding and test APIs
