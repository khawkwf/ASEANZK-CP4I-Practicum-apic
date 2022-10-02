# ASEANZK-CP4I-Practicum-apic
CP4I API Connect enablement 
### Tutorial Overview
<img width="698" alt="image" src="https://user-images.githubusercontent.com/25983259/193075472-7df79cae-05e1-49bb-946c-258aaab1e55f.png">

### Tutorial Use Case:

A regional bank wants to move to the cloud and adopt a platform to share data among registered partners but is concerned about continuing to meet regulatory compliance standards and mitigating potential security risks. As their integration specialist, you have been asked to demonstrate that access to account information via APIs all is secure no matter where they reside, and that the sensitive data MUST be protected. In this example, you’ll use security-rich features within the API management lifecycle to help proxy an API and mask sensitive data from the response, using built-in policies
and mitigating potential security risks.

In this tutorial, you will be using the API Connect tooling to design and meet the following business requirement.

  - Bank needs to provide customer account information to digital channels. e.g Fintech

  - Create an API to allow digital channel developer to access & test the API from Bank’s developer portal.

  - Customer account information is stored in core banking system with the virtual API endpoint: 
(https://cp4i-mock-api.mybluemix.net/accounts)

  - Sensitive data such as Identity Card No. & Phone No. must be masked before sending response back to the consumer app.

  - Define API Plan with the quota,  e.g. 100 calls per minute

  - Only the authorized consumer app can consume the API with valid Client ID key.
  
  - Need to authenticate and authorize the end customer using OAuth Token before accessing the core banking API.
  
  - Any version changes to the backend API should not affect the existing API subscribers. 
  
  
### Lab Summary:
  
- Lab 1: Bank developer persona - Create API for external channel to access

- Lab 2: Bank admin persona -  Define API Plan and Publish API

- Lab 3: Consumer organisation developer persona - Self Service on-boarding, subscribe and consume APIs

- Lab 4: Bank security persona - Define API security policies

- Lab 5: Bank admin persona - Manage API versions and life cycle

- Lab 6: Bank business personal - Analyze API usage and statistics




### Tutorial Context:

<img width="606" alt="image" src="https://user-images.githubusercontent.com/25983259/193087744-8569dce7-bb37-4be6-9d8a-1be8264dc287.png">


#### 1: Create API 

1.1 Access IBM Cloud Pak console and choose to login using "IBM provided credentials (admin only)"

![image](https://user-images.githubusercontent.com/25983259/193182455-fdd827f5-10e0-4381-9a76-e6fe5843baba.png)
<img width="1295" alt="image" src="https://user-images.githubusercontent.com/25983259/193182833-623e9705-1321-4deb-8e7c-d941b4184128.png">

1.2 Navigate to "Design APIs"

![image](https://user-images.githubusercontent.com/25983259/193183286-6f2def80-795a-4d4c-b2e1-09c4126820a5.png)

1.3 You’ll be asked to log into the API Manager if you have not logged in before. Click on "IBM Common Services user registry". You should be logged in automatically using SSO. If not, use "admin" and the same password you used to log into the cloud pak home page.

![image](https://user-images.githubusercontent.com/25983259/193183535-3c5860c6-0a12-4ec7-a67f-65116fc663b8.png)

1.4 You’ll arrive at the API Manager home page here

<img width="1063" alt="image" src="https://user-images.githubusercontent.com/25983259/193183616-f890bb0a-b99d-4b8d-828c-ea25797cf0fe.png">

This is where we can edit, assemble, secure and test our APIs and move products through their lifecycle, and manage the availability and visibility of APIs and Plans.

1.5 Now let's define a new REST API. 

On the Home page, click the “Develop APIs and products” tile.

![image](https://user-images.githubusercontent.com/25983259/193184647-645e86a2-0596-4b0c-b664-71e6069c535b.png)

1.6 On the Develop page, click “Add > API (from REST, GraphQL or SOAP)”.

![image](https://user-images.githubusercontent.com/25983259/193184731-8bf5e92c-1e40-4c13-9f5b-a8159936b3fd.png)

1.7 In the Import section, select “OpenAPI 3.0” tab and “From target service”. This will create a REST proxy to secure the target API endpoint. Click “Next”.

![image](https://user-images.githubusercontent.com/25983259/193184890-8913badd-45a1-489b-9225-87067b3cba83.png)

1.8 Enter the following information to create a REST API from a target service.

- In the “Title” field, enter "accounts-xxx". Note: xxx is your name as suffix to avoid overlapping with other student's API.
- The “Name” field is auto-populated with accounts.
- Leave the “Version” field as 1.0.0.
- The “Base Path” field is auto-populated with /accounts.
- In the “Target Service URL” field, enter a live API endpoint, e.g. "https://cp4i-mock-api.mybluemix.net/accounts"

<img width="1039" alt="image" src="https://user-images.githubusercontent.com/25983259/193186012-331da201-c06c-44bd-9451-50d165a7dcf3.png">

1.9 Click “Next”.
In the security dialog, by default the following specification will be enabled.
- “Secure using Client ID”: This will enforce API key security definition which specify that an application must provide a client ID to call your API.
- “CORS”: This will enable cross-origin resource sharing (CORS) support for your API. CORS allows embedded scripts in a web page to call the API across domain boundaries.

1.10 Click “Next”.
![image](https://user-images.githubusercontent.com/25983259/193186231-c0a26144-ceb7-46fe-81e8-4305059fee46.png)

1.11 The new API is created, and the Summary page is displayed. Click “Edit API” to continue
configuring the API.

![image](https://user-images.githubusercontent.com/25983259/193186397-6d1e1245-18f0-4199-b72c-0b0e54803a18.png)
API Design page is displayed.

1.12 Click 'Gateway' tab.

![image](https://user-images.githubusercontent.com/25983259/193186744-96462afc-cb27-4e94-bcd7-9ec406c47fbf.png)

1.13 From the action palette on the left under “Transforms”, drag-and-drop the “Parse” action onto the assembly flow line, to the right of the "Invoke" icon. The configuration panel opens automatically. Retain the default settings. This will enable the flow to detect the response message structure from the invoke action as a JSON string and parse it as a JSON.

![image](https://user-images.githubusercontent.com/25983259/193187092-d9f36a63-1fde-4097-b38e-a90b5d595c02.png)

1.14 From the action palette on the left under “Transforms”, drag-and-drop the “Redaction” action onto the assembly flow line, to the right of the "Parse" icon. The configuration panel opens automatically. You can maximise it by clicking maximize icon. We want to mask any “Identification” and “Phone” occurrences from the entire message returned from the invoke action in order to block out sensitive data returned to the client.

![image](https://user-images.githubusercontent.com/25983259/193187615-e91e57f6-cc48-4313-8483-10a00a6d2125.png)

1.15 Specify the following JSONata path expression for the “Path” policy property to redact all occurrences of the “Identification” field in response data and choose “Redact” action.

![image](https://user-images.githubusercontent.com/25983259/193188699-f642d2df-69dd-4a69-93f2-bba5f09524c3.png)

1.16 Click “Add action” to specify another JSONata expression for an additional field to be
redacted.

![image](https://user-images.githubusercontent.com/25983259/193188733-f9575a02-7f6c-4a14-9c0b-ea47513ca3b6.png)

1.17 Use the following JSONata path expression for the “Path” property and choose “Redact”
action.

![image](https://user-images.githubusercontent.com/25983259/193188990-bc433754-563d-4412-9464-93afd75dc23b.png)

1.18 Click the blue “Save” button on the upper right.

1.19 Now let's test the API.  

Move the activation slider control to the online position to activate your API. Activating the API automatically progresses the api lifecycle for you in the Sandbox catalog.

![image](https://user-images.githubusercontent.com/25983259/193189334-7aa2a8b5-e85c-4a51-b8a5-eda219c6aa95.png)

The activation step above actually performs the following tasks internally:
- Creates the default Product and Plan and publishes the Product to the Sandbox Catalog so
that the API is available to be called.
- Subscribes the Sandbox test application to the Product so that you can immediately test the
API in the test environment.

1.20 Also notice that “Test bar” appears.

![image](https://user-images.githubusercontent.com/25983259/193194397-df3b8810-8e9e-43d7-84ad-a8e5d817e646.png)

1.21 Click the “Test” tab of the accounts API.

In the Operation section, select the get /accounts operation to invoke. On the parameters tab, default header parameters and values are already provided for you. As our API has client ID definition applied, the corresponding key is added as header parameters, with the values
present.

<img width="1026" alt="image" src="https://user-images.githubusercontent.com/25983259/193194556-dac06938-8ac2-46ab-90ad-82615ba1e977.png">

1.22 Click “Send”. The API response is displayed in the Response section. Notice that the values of
“Identification” and “Phone” fields are blocked out.

![image](https://user-images.githubusercontent.com/25983259/193194838-a0bc659e-7b72-4e17-9ed9-2cc06b2dd871.png)

NOTE: You may get this instead; a message relating to an untrusted certificate. If you do, follow the instructions and click the link provided, accept the certificate. You may then see ‘401’ error – this is fine – your browser is sending the request without any credentials. Go back to your
original test tab and try the request again.

![image](https://user-images.githubusercontent.com/25983259/193194897-6efc6db3-58ab-4cfd-86c8-0e94e39223dd.png)

1.23 Click the “Trace” tab to display a record of the API execution so you can see what actions were triggered and the code that we executed for each action. In the diagram, the policies (actions) that were executed during the call are highlighted. 

1.24 Select the "invoke" or "parse" or “redact” policy to examine its trace for each action within the API flow. You will find in the code box some useful
information about how many times redact action is executed along with latency and memory usage performance metrics.

![image](https://user-images.githubusercontent.com/25983259/193195270-f977ec2f-319b-47cc-9f43-bf11bffa6944.png)

Congratulations! You have created and tested your first API flow; proxied a backend application, secured it with an API key and masked sensitive data.
Now that you have learned how to design and secure APIs, why don't you try doing it on your own by proxying your own backend API :)



#### 2: Publish API

In this lab, we will make the API available to developers. In order to do so, the API must be first put into a product and then published to the Sandbox catalog. A product dictates rate limits and API throttling. When the product is published, the Invoke policy defined in the previous lab is written to the gateway.

2.1 From the vertical navigation menu on the left, click "Develop".

![image](https://user-images.githubusercontent.com/25983259/193212303-c2792ff2-c102-4ef5-a8bf-4056d346443b.png)

2.2 Click the "Products" tab.

![image](https://user-images.githubusercontent.com/25983259/193208291-ab2a4761-4dc9-4558-8ae9-5994e5a31a15.png)

2.3 Click "Add" and click "Product" from the drop-down menu.

![image](https://user-images.githubusercontent.com/25983259/193208726-44afb4c4-3ded-493a-867d-edc196902564.png)

2.4 Click "New product" and click Next.

![image](https://user-images.githubusercontent.com/25983259/193208917-ee388fe8-c714-454b-965e-77b17c6f90bd.png)

2.5 Enter "Customer-xxx" for the Title and click Next. Note: xxx is your name as suffix to avoid overlapping with other student's product.

<img width="801" alt="image" src="https://user-images.githubusercontent.com/25983259/193209568-2ecc8425-4ece-45d3-b63e-3b4334c613f3.png">

2.6 Select the "Account-xxx" API and click Next.

![image](https://user-images.githubusercontent.com/25983259/193210735-66f9f302-d477-4a1b-b637-fddf57897725.png)


2.7 Keep the Default Plan as is and click Next.

![image](https://user-images.githubusercontent.com/25983259/193210632-654ad176-06bb-43b7-8972-335fd18b2547.png)

2.8 Select "Publish product" and confirm that "Visibility" is set to Public and "Subscribability" is set to Authenticated. Click Next.

![image](https://user-images.githubusercontent.com/25983259/193210914-5670422b-462c-41a5-99a5-f77180067bc3.png)

2.9 The Product is now published successfully with the API base URL listed and available for developers from the Developer Portal. Click Done.

<img width="1257" alt="image" src="https://user-images.githubusercontent.com/25983259/193211456-7e655673-c5f2-4277-a362-6027c253c508.png">


Summary

Congratulations, you have completed the Create and Secure an API lab. Throughout the lab, you learned how to:
• Create an API by invoking a mocked backend service (account service)
• Configure ClientID/Secret Security, endpoints, and proxy to invoke endpoint
• Test a REST API in the Developer Toolkit
• Publish an API for developers into Sandbox catalog


#### 3: Subscribe & Consume API

In thie lab, you are playing a role as consumer organization developer. You are interested to explore what APIs are made available in the Bank API developer portal, which was successfully published in your previous lab exercise.

3.1 Open a Developer Portal based on the portal URL given.

![image](https://user-images.githubusercontent.com/25983259/193216476-012ef906-2110-4bf1-8883-a1783baf7c0d.png)

3.2 Click "Create account" and complete the sign up form. Specify the name of the Consumer Organisation, e.g. Fintech-Kevin

![image](https://user-images.githubusercontent.com/25983259/193219474-0cb73b13-fc5c-49b1-a933-dc463265549f.png)

3.3.Click Sign up. A confirmation message is sent to the email address provided in the form.

3.4 To use the Developer Portal, click "Sign in" and login with the user credentials you specified.

![image](https://user-images.githubusercontent.com/25983259/193220193-ad50b41f-0087-4254-9b00-7dc5e88854c3.png)

3.5 Upon login, you will see 5 key functions displayed in the top menu bar:
a. API Product
b. App
c. Blogs
d. Forum
e. Support

![image](https://user-images.githubusercontent.com/25983259/193220736-7ebfca73-6dfe-4dca-ad92-43a8cc75b0f5.png)

3.6 Simply click on each of the above to view the capability.

3.7 Now let's create and register a new App in the developer portal.

In the Developer Portal, click "App" -> "Create new App".
Note: You cannot register new applications if you are logged in as the administrator.

![image](https://user-images.githubusercontent.com/25983259/193221401-28261089-3e1b-4cd9-baa4-42b6e5c3f69a.png)

3.8 Enter the Title of the application, e.g. Application-xxx. Note: xxx is your name as suffix to avoid overlapping with other student's application.

<img width="549" alt="image" src="https://user-images.githubusercontent.com/25983259/193223731-7f35a537-cef9-489b-9178-0474341325e8.png">

3.9 Click Save.

3.10 Copy the key and secret values for the new app. Click OK.

![image](https://user-images.githubusercontent.com/25983259/193223305-a49ecf56-dfff-41f2-9124-6e11455e62c3.png)

3.11 Click OK.
You see the Subscription summary page for the App your have just registered

![image](https://user-images.githubusercontent.com/25983259/193224856-6cf415c3-041c-42dd-9c9c-11ccf610eef5.png)

3.12 You may explore what is inside by clicking on “Dashboard” and “Notification” menu:

![image](https://user-images.githubusercontent.com/25983259/193225111-5b581fae-b247-4b8a-973d-35879f19d105.png)

3.13 Next, let's browse and start subscribing to the API products. 

3.14 Click API Products. Then search for the API Product (e.g. Customer-xxx that was created in previous lab.

![image](https://user-images.githubusercontent.com/25983259/193225790-b7671b6a-7ef7-4829-9351-7cc2c072749a.png)

3.15 Click the Customer-xxx product and you will see the product details such as APIs and Plans available for this product, as shown below:

<img width="575" alt="image" src="https://user-images.githubusercontent.com/25983259/193227019-70da84b4-5827-47a5-ba37-a941a6514227.png">

3.16 Choose the default plan by clicking the “Select”

![image](https://user-images.githubusercontent.com/25983259/193227467-d72f8b01-2238-4541-8ddb-5a9941d0b302.png)

3.17 You will go through the API product plan subscription steps as shown below.

![image](https://user-images.githubusercontent.com/25983259/193227753-f58c6a3f-94fe-4420-b734-1e82aaaf2aad.png)

3.18 Select an existing app that you had registered previously: e.g. Application-xxx 

![image](https://user-images.githubusercontent.com/25983259/193228421-3f188962-1d0c-4f7e-a096-6440f6d9b169.png)

3.19 Click Next. 

![image](https://user-images.githubusercontent.com/25983259/193228800-ecbeb8dd-50db-44a4-8ea0-4e764860b765.png)

3.20 3.19 Click Done.

![image](https://user-images.githubusercontent.com/25983259/193229188-fe5de77a-d50e-4ba1-ad56-c60bb5778e9f.png)

3.21 At this stage, your Fintech app have successfully subscribed to Customer product which contains Account API and plan.

3.22 Next, let's test the API you have just subscribed.

3.23 Click API Products in the Developer Portal dashboard.
![image](https://user-images.githubusercontent.com/25983259/193384068-3cee1504-365e-4ce4-b454-88c615b3c76d.png)

3.24 Click the product such as "Customer-xxx" then select the "Accounts-xxx" API from the list.

![image](https://user-images.githubusercontent.com/25983259/193384362-62a4c17e-8b1f-44f4-9e96-cce40ef9e994.png)

3.25 Click "GET /"

![image](https://user-images.githubusercontent.com/25983259/193384373-418b46d8-a8a0-4806-81ff-0aa86b2c5c6c.png)

3.26 Click "Try it"

![image](https://user-images.githubusercontent.com/25983259/193384419-1a0fb7b7-a7e4-48e2-96ab-92b914eca718.png)

3.27 Click "Send"

Note: If no response is received, navigate to the URL that is displayed at the beginning of the Try this operation section, in a new browser tab. Accept the security certificate, and then call the operation again.

3.28 A returned response of 200 OK and the message body are displayed, indicating that the REST API operation call was successful.

![image](https://user-images.githubusercontent.com/25983259/193384492-53cde3c7-3d78-4efe-b0d4-0996c381ab4d.png)


Summary

Congratulations, you have completed the 'Self Service on-boarding, Subscribe and Test API" lab. Throughout the lab, you learned how to:

• Created a developer account in the Developer Portal.
• Created and registered a new App, and subscribed it to a Plan.
• Tested an API in the Developer Portal.

For more advanced labs, you may refer to the advanced portal tutorial:
https://www.ibm.com/docs/en/api-connect/10.0.5.x_lts?topic=apis-developer-portal-tutorials

• User authentication
    Adding a field to the sign up form
    Adding validation to a field on the sign up form
    Adding a field group to group fields in a content type
   
• Configuring the Developer Portal
    Creating the Portal
    Creating a private Portal site
    Customizing themes
    
•  Advanced customization
    
    Changing the front page
    Grouping products by category
    Adding a custom block to the front page
    Adding a custom block to a page other than the front page
    Using avatars
    Using image optimization
    Adding a Back to top button
    Adding a zoom effect
    Creating a drop-down menu link
    Displaying blog posts on the front page
    Displaying tweets on the front page
    Changing the profile page to display firstname lastname, instead of username
    Using a custom weighting sort order on the product list page
    Configuring the RobotsTxt file
    Configuring the SecurityTxt file
    Synchronizing application credentials with an external server


#### 4: Secure API

API Connect is a full-featured OAuth 2.0 provider. The OAuth exchange works like any other API call, and thus we treat it as its own API. In this section, you will create a new OAuth provider API, configure which grant type to use, and configure how it will authenticate user credentials.

In order to configure user authentication, you must first define the registry to use, which may be LDAP, local user registry, or an authentication URL. For this lab, we will implement an Authentication URL.

4.1 In the API Manager from the main menu on the left, click "Resources".
![image](https://user-images.githubusercontent.com/25983259/193392220-c38e76c4-9b65-4910-892f-0a61b1c0e33c.png)

4.2 In the 'User Registry' page, Click 'Create' 

![image](https://user-images.githubusercontent.com/25983259/193392411-4f9f3a04-912d-4003-8e5d-9189ebc6c3fc.png)

4.3 Select 'Authentication URL User Registry'

![image](https://user-images.githubusercontent.com/25983259/193392512-74ba1e08-63fe-4420-bb87-2ba021a6a854.png)

4.4 Specify the only following properties and then click [[Save.]]

Title: [[App Registry]]

URL: https://thinkibm-services.mybluemix.net/auth

Display name:[[ App Registry]]

Click Save to save the resource

<img width="1073" alt="image" src="https://user-images.githubusercontent.com/25983259/193393090-d5e9fef5-6427-44e6-9272-afdc1d4b409f.png">

4.5 Next, are are going to add the oAuth provider. In the Resources menu, under 'OAuth Provider' section, click [[Add->Native OAuth Provider]].

![image](https://user-images.githubusercontent.com/25983259/193393235-4c3774f0-3c34-4fe6-98f2-7e4002b13251.png)

4.6 Specify the following properties and click [Next] to continue.

Title: [[oauth]]

Name: [[oauth]]

Gateway Type: [[DataPower API Gateway]]

<img width="1094" alt="image" src="https://user-images.githubusercontent.com/25983259/193393281-e735d7bb-41db-4aef-a761-98ba4f893e0c.png">

4.6 The next configuration screen will display the default paths to the Authorize and Token functions. For Supported grant types, choose [[Resource owner password]]. For Supported Client types, choose [[Confidential]]. Click [[Next]] continue.

<img width="1065" alt="image" src="https://user-images.githubusercontent.com/25983259/193393331-005edd85-cb64-497d-8d6b-7d9f7669dab2.png">

4.7 One scope is generated for you : [[sample_scope_1]]

<img width="1094" alt="image" src="https://user-images.githubusercontent.com/25983259/193393406-ee205571-62d0-497f-b537-3b697ad1a6eb.png">

4.8 Modify the values for [sample_scope_1], set the following fields:

Name: [[account]]

Description: [[Access to Accounts API]]

![image](https://user-images.githubusercontent.com/25983259/193393450-d2f3807a-fd6a-452a-b914-c16aca79ddf4.png)


4.8 Click [[Next]].

<img width="1095" alt="image" src="https://user-images.githubusercontent.com/25983259/193393496-bf4109f2-ac1a-4d74-bc71-999237179370.png">

4.9 Keep all items default. Click [[Finish]]

![image](https://user-images.githubusercontent.com/25983259/193393528-aa235d12-51db-4ba3-81c2-1ccc84fdc534.png)

4.10 Review your OAuth configuration and click [[Save]].
 
![image](https://user-images.githubusercontent.com/25983259/193393610-a2259f33-6e5a-4642-82d6-73584c2d3f5e.png)

Now, you have already setup the Native OAuth Provider.

4.11 From the Sandbox Catalog registry setting, select API User Registries and Add App Registry. To open Sandbox Settings follow Home->Manage Catalogs->Sandbox->Catalog Settings->API User Registries.

<img width="1440" alt="image" src="https://user-images.githubusercontent.com/25983259/193393894-314df957-d73f-4f63-a7ca-c5cf793af22b.png">

4.12 Use Edit option and enable available API registry. Then cick Save

<img width="1025" alt="image" src="https://user-images.githubusercontent.com/25983259/193393983-144d4eab-c969-4cc8-9bdf-57a848c32756.png">


4.14 Now, we need to add the OAuth Service to the Sandbox Catalog. Under the Sandbox->Catalog Settings, go to 'OAuth Provider'

<img width="1351" alt="image" src="https://user-images.githubusercontent.com/25983259/193394185-47e97854-c254-442f-a149-3ad08284cc0e.png">

4.15 Click Edit and Choose the OAuth service created above. Then click [[Save]].

<img width="1391" alt="image" src="https://user-images.githubusercontent.com/25983259/193394307-658d99bb-43e7-4015-b0e1-3c8af2a13dc3.png">

4.16 Next we are going to secure your account-xxx API with OAuth. 

API Connect supports multiple versions of APIs. Create a new version of the accounts API before making any changes that would break functionality for existing consumers. 

4.17 In the API Manager from the main menu on the left, click [[Develop]].

![image](https://user-images.githubusercontent.com/25983259/193395435-132f77d2-4c60-410d-aa30-31ae42002231.png)

4.18 Click on the menu icon to the right of [accounts-xxx 1.0.0] API and select [[Save as new version]].  

![image](https://user-images.githubusercontent.com/25983259/193395521-e307f389-18ef-462b-bdb6-e087bc957853.png)

4.19 Enter the new version number as [[2.0.0]] and click [[Submit]].

<img width="1034" alt="image" src="https://user-images.githubusercontent.com/25983259/193395564-f65c1709-c8c1-437f-bb01-bbf131c24309.png">

4.20 Next, we are going to add OAuth security to the new version of accounts-xxx API. 

4.21 From the Develop home page, click `accounts-xxx 2.0.0`. Navigate to the [Component->Security Schema] section.

![image](https://user-images.githubusercontent.com/25983259/193396219-9e55ac52-4f25-4f56-ae39-e243444ff441.png)

4.22 Click [[Add]].

On the API Security Schema screen, enter the following:

    Name: [oauth-1]

    Description: [[API OAuth security definition]]

    Type: [[OAuth2]]
    
    OAuth Provider Name: [[oauth]] 

    Flow Type : [[Resource owner]]

    Leave everything else to the default values and click Save.  

<img width="680" alt="image" src="https://user-images.githubusercontent.com/25983259/193397096-e689beb4-aa64-4e3b-8568-0216091112ad.png">

4.23 Review the OAuth Schema added in this accounts-xxx 2.0.0. API. 

4.24 Click Save.

![image](https://user-images.githubusercontent.com/25983259/193398552-c2f9b549-ab0c-4e18-b1d2-ea05b9b22c9f.png)

4.25 Navigate to the `Security` section and click 'Add' to add the OAuth schema.

![image](https://user-images.githubusercontent.com/25983259/193398760-255aa49c-de61-44d7-ad73-7b31877adad6.png)


4.26 Check the `oauth-1` checkbox. Make sure `account` scope is also checked. Then click 'Create'

<img width="710" alt="image" src="https://user-images.githubusercontent.com/25983259/193398852-6fad7f17-1d23-4663-b439-f4ac36a5d341.png">


4.27 Click 'Save', then turn the API to online mode.

![image](https://user-images.githubusercontent.com/25983259/193398910-3d049787-8470-4543-8d82-b3ab987d0e68.png)

Summary:
You completed Lab 4 - Add OAuth Security to your API. Throughout the tutorial, you explored the key takeaways:

• Configure an OAuth 2.0 service, the Resource Owner Password grant type.

• Clone a new version of an API.

• Secure the new version of your API.


#### 5: Manage API
In the previous lab, you created a new version of the Account-xxx API which is secured with an OAuth 2.0 provider. At this stage, however, the changes are still in draft mode. In order for the changes to take effect, you must publish the APIs to the developer portal and make them available for the API Consumers. Recall though that the account-xxx 1.0.0 version is already running and has active subscribers.

API lifecycle management capabilities is an essential part of the API Management platform. The API lifecycle includes the following stages:

• Plan and design the API.

• Develop the API.

• Test the API.

• Deploy (publish) the API.

• Retire and deprecate the API.

In this tutorial, you will explore the following key capabilities:

• Creating a new API Product

• Replacing the Old Product

• Testing Your OAuth API in the Developer Portal

Create A New Product:
In IBM API Connect, Plans and APIs are grouped together in Products, with which you can manage the availability and visibility of APIs and Plans. Products allow related APIs to be bundled together for subscribers. In Lab 1, when the accounts-xxx API was created, it also created a default product. In this section, you will create a new product from scratch and stage it to your API Manager environment.

The following link provides more information about API Products in IBM API Connect:

https://www.ibm.com/support/knowledgecenter/en/SSMNED_v10/com.ibm.apic.toolkit.doc/capim_products.html

5.1 Now let's create a new product. Go to the [Develop] tab in your API Connect manager interface.

![image](https://user-images.githubusercontent.com/25983259/193399715-670c10d4-5dd8-4706-875f-8aeb0e91a584.png)

5.2 Click [Add] and select [Product]

![image](https://user-images.githubusercontent.com/25983259/193399747-c5074fab-7592-4fd9-8b9c-0f9f13e8aedd.png)

5.3 Click [New product] and then click [Next]

<img width="1083" alt="image" src="https://user-images.githubusercontent.com/25983259/193399770-9bb70fc2-773c-4a60-ba1b-6f244d578a3d.png">

5.4 Title this product [[Customer-xxx-with-SSO]] and click [[Next.]]

<img width="1081" alt="image" src="https://user-images.githubusercontent.com/25983259/193401009-a31b587c-2a70-4622-a2df-df3870160de7.png">

5.5 Click 'Next'

5.6 Select the 'account-xxx' 2.0.0 version, which is the version that you have enabled with OAuth2.0 security policy in previous lab.

![image](https://user-images.githubusercontent.com/25983259/193401116-850a8519-69ae-4292-98ee-e834d311b277.png)

5.7 Click 'Next' to accept the default plan.

<img width="1081" alt="image" src="https://user-images.githubusercontent.com/25983259/193401178-c6be5540-2801-4130-8a5f-4e5e52c631b4.png">

5.8 Click 'Next' to define the publish policy. Keep as default.

<img width="1076" alt="image" src="https://user-images.githubusercontent.com/25983259/193401274-34231cee-75ce-4782-accb-12158c67045f.png">

5.9 The new product 'Customer-xxx-with-SSO' is now created. Click 'Done'

<img width="1093" alt="image" src="https://user-images.githubusercontent.com/25983259/193401375-3966c874-7a71-41e6-9526-deca104d4d98.png">

5.10 Click the new product just created 'Customer-xxx-with-SSO'. You may want to browse across other sections of the *API Product* menu such as *Visibility*, *Plans*, and *Categories* to explore additional API Product management capabilities provided by IBM API Connect. 

![image](https://user-images.githubusercontent.com/25983259/193439223-42787e44-62cd-46ad-b571-464211a7a411.png)

5.11 Just to confirm the new product 'Customer-xxx-with-SSO' is packaged with 'account-xxx 2.0.0' version.

![image](https://user-images.githubusercontent.com/25983259/193439313-cdf2cf97-b5f7-415b-bc63-824899720278.png)

5.12 Based on the information provided, API Connect will generate a yaml definition of your API Product. You can check it by clicking on the [Source] button at the top.

![image](https://user-images.githubusercontent.com/25983259/193439445-ed775a8e-d732-4475-b084-7a44d0a5278f.png)


5.12 Next, let's stage the Product to your API Manager environment

Before an API Product can be published, you must first stage that Product to a Catalog. When a Product is in the staged state, it is not yet visible to, or subscribable by developers. However, it can be reviewed by the API Product Manager and Published once it is decided that the API Product is ready to be consumed.

5.13 In the API Manager, from the main menu on the left, click [Develop].

5.14 Click the [⋮] button next to your API Product 'Customer-xxx-with-SSO' and [Stage] it.

![image](https://user-images.githubusercontent.com/25983259/193439703-8356c468-0ec0-41ff-9037-15ba96ce308a.png)

5.15 Choose your [Sandbox] catalog as a target for Staging. and click 'Next'

Note:

IBM API Connect allows you to publish products to specific gateways associated with the Catalog.

![image](https://user-images.githubusercontent.com/25983259/193439741-fc7008c0-5d42-4d3e-8ee4-78f5b18472a9.png)

5.16 Click 'Stage' 

![image](https://user-images.githubusercontent.com/25983259/193439897-91e90e4f-f456-4d32-9237-dc59b1dbbb41.png)

5.16 Next, let's supercede the Old Product

IBM API Connect provides capabilities for managing the lifecycle of your API Products. There are various states which an API Product can reside in, as well as controls around when you can move an API Product from one state to another. In this section, you will explore how to replace a running version of an API Product with a new one.

5.17 Switch to the [Manage] tab of the interface and click on your [Sandbox] catalog tile.

![image](https://user-images.githubusercontent.com/25983259/193440107-d05d41bf-8d11-4370-8ceb-c40021a12aae.png)

5.18 The [Products tab] will list all of the API Products that this Catalog is currently managing.

Make sure your newly created API Product [Customer-xxx-with-SSO' Products] is in the [Staged] status while the old [Customer-xxx] product is [Published] in the Catalog.

![image](https://user-images.githubusercontent.com/25983259/193440135-6d74562e-8b92-4f19-9d85-6ba22b1bcf85.png)

5.19 Click on the menu options for the existing published product [Customer-xxx] and select the [Supercede] option.

![image](https://user-images.githubusercontent.com/25983259/193440797-dd3f6e0c-bc91-481a-9b71-c222c43e36ea.png)

5.20 Select the newly staged product [Customer-xxx-with-SSO] to supercede [Customer-xxx]

![image](https://user-images.githubusercontent.com/25983259/193440858-843d8276-2214-47aa-8697-10716e82fc17.png)

When you supercede a product with another product, the following actions are taken: 1) the superseding product is published, 2) the visibility and subscribeability settings from the original product are used in the superseding product, and 3) the original product is moved to the deprecated state. When a product is deprecated, application developers that are already subscribed to the product can continue to use it, but no new developers can subscribe to the product. A deprecated product can be re-published if required.
5.21 Click 'Next'

5.22 In order to maintain our consumers’ entitlements, we need to migrate their plan subscriptions.

Both of our Products have plans called [Default Plan.] You will now choose to move subscribers from the [Customer-xxx] Product’s default plan to the [Customer-xxx-with-SSO]’s default plan.

5.23 In the drop-down menu, select Default Plan and then click [[Supercede]].

![image](https://user-images.githubusercontent.com/25983259/193441028-36c917af-44eb-4668-b19b-2a48c9417d89.png)

5.24 API Connect will take care of deprecating the old product and publishing the new one. As a result, the new 'Customer-xxx-with-SSO' Products product will be published, while the old one 'Customer-xxx' will be automatically deprecated.

![image](https://user-images.githubusercontent.com/25983259/193441294-b15325d1-dc3e-4f83-9d17-3bd723ce51b1.png)

5.25 Now, let's see the result of the API product superseding in the API developer portal.

5.26 Open your API Portal in a new browser tab and log in with your developer account. 

5.27 Click the [API Products] tab.

Notice that the old [Customer-xxx] product is no longer available. It has been replaced by your new [Customer-xxx-with-SSO] product.

![image](https://user-images.githubusercontent.com/25983259/193441504-c192e4c6-e5f5-421a-a613-1e0ca7a46017.png)

5.28 Click on the [Customer-xxx-with-SSO]. 

5.29 Select the default plan as migrated from old product to this new product.

![image](https://user-images.githubusercontent.com/25983259/193442128-d9054370-e57f-4b24-b004-62aab5f43c6c.png)

5.30 Select the same application created last time, i.e. [Application-xxx]

![image](https://user-images.githubusercontent.com/25983259/193442166-f2ab25a3-7945-4605-816e-25438dca3519.png)

5.31 Proceed to subscribe to this new product by clicking 'Next'

![image](https://user-images.githubusercontent.com/25983259/193442229-62487324-fff3-4b03-abfe-161e7055d763.png)

5.32 Click 'Done'

Now, we can proceed to test the newly subscribed product that consists of the account-xxx 2.0.0 that has the OAuth security policy.

5.33 Click on the 'account-xxx 2.0.0' API

![image](https://user-images.githubusercontent.com/25983259/193442430-ebcb0ffb-dc1c-40aa-9c98-172207e7555f.png)

5.34 Select the [GET /] operation. Notice that we now have an additional OAuth security requirement defined.    

![image](https://user-images.githubusercontent.com/25983259/193442502-6532de5e-5951-4d17-82d9-612b7bfc17f3.png)

5.35 Click on 'Try it'

5.36 Choose the 'clientID, oauth-1' and input the required identity information:

- Select your subscribed application from the [Client ID] drop-down menu.

- Paste your secret into the [Client secret ]field.

- In the [Username] and [Password] fields, you can enter any text. Select the 'account' scope. Note:  Recall that when we configured the OAuth API, we provided an Authentication URL as the method for validating the user credentials. The URL that we provided will respond back OK with any credentials.

- Click the [Get Token] button to obtain an OAuth token. 

The API Portal will call out to the OAuth Token URL with your client credentials and user credentials.
The OAuth API which you built in lab 3 will intercept the request, validate the credentials, and generate a token.  

![image](https://user-images.githubusercontent.com/25983259/193443135-3c69013c-75ac-4f64-bc1d-e32f7f8a5ee9.png)

5.37 You should receive the response after successfully authenticated and authorized by API Gateway based on the security policy configured.

![image](https://user-images.githubusercontent.com/25983259/193443216-a178c8cf-78e5-47e4-a9f2-32e6881c94fb.png)

Summary

You completed Lab 5 - Use Lifecycle Controls to Version your API. Throughout the tutorial, you explored the key takeaways:   

• Create a new API Product.

• Supercede the Old Product.

• Test Your OAuth API in the Developer Portal.

#### 6: Analyze API

You can use API Connect to filter, sort, and aggregate your API event data. You can present the results within correlated charts, tables, and maps to help you manage service levels, set quotas, establish controls, set up security policies, manage communities, and analyze trends.

API analytics is built on the OpenSearch open source real-time distributed search and analytics engine.

The content of the API event data depends on the logging policy that is set for the operation. For more information about the fields that are displayed in an API event record, see API event record fields. For information about how to configure your logging preferences for API events, see activity-log policy and Including elements in your assembly.

6.1 To access the API Analytics data via API Manager Dashboard, Navigate to [Analytcs] tab in your API Connect manager interface.

![image](https://user-images.githubusercontent.com/25983259/193446190-e5d5003f-1aa5-4538-9acd-2acbe12acaeb.png)

6.2 Five out-of-the-box dashboards are available:

• API Dashboard
• Product Dashboard
• Monitoring Latency Dashboard 
• Monitoring Status Dashboard 
• Usage Dashboard

<img width="1224" alt="image" src="https://user-images.githubusercontent.com/25983259/193446450-5bc06002-5dc3-4851-8248-7fb64be684e1.png">

6.3 Let's examine each dashboard to view next level of analytis details. 

6.4 Click on 'API Dashboard'. This dashboard contains charts that summarize total API calls, response codes, and response times.

<img width="718" alt="image" src="https://user-images.githubusercontent.com/25983259/193446640-e5460c7c-22a1-4950-b288-3dcc76cbb131.png">

Note: Each chart in the dashboards can be exported as JSON, CSV files, or as PNG or JPG images. To do this click the actions button at the top-right:
It is also possible to enlarge the chart to full-screen by clicking Analytics full screen chart icon. To see the source data in tabular form click Analytics table view icon.

6.5 Click 'Back' to return to the main dashboard view.

6.5 Click on 'Product Dashboard'. This dashboard contains charts that show total API calls and Application subscriptions per plan.

<img width="1171" alt="image" src="https://user-images.githubusercontent.com/25983259/193446855-37fb7077-cf0c-4388-8488-dc24eca63f10.png">

6.6 Click 'Back' and explore 'Monitoring Latency Dashboard'. This dashboard contains charts that provide response time statistics and data usage.

<img width="768" alt="image" src="https://user-images.githubusercontent.com/25983259/193447082-d22c22a8-b0f7-4be0-b2a8-d03d4539f68c.png">

6.7 Click 'Back' and explore 'Monitoring Status Dashboard'. This dashboard contains charts that show the response codes and success/failure rates of API calls.

<img width="668" alt="image" src="https://user-images.githubusercontent.com/25983259/193447109-d44fb8fd-8e55-4e70-82df-210bd83cbb69.png">

6.8 You may further drill down the API event details of each API call either on error or success, by clicking on each API event listed.

![image](https://user-images.githubusercontent.com/25983259/193447265-c5654335-aa2c-4e65-9c9d-60016ed1347b.png)

You may examine the latency of each API actions within the assembly flow.
<img width="1065" alt="image" src="https://user-images.githubusercontent.com/25983259/193447303-35d0f680-8c4f-400b-bb9a-af269532e361.png">

You can also examine the API event data of each API invocation.
<img width="770" alt="image" src="https://user-images.githubusercontent.com/25983259/193447436-d988deb3-b311-4896-9de0-f9dca3a727dd.png">

6.8 Click 'Back' and explore 'Usage Dashboard'. This dashboard contains charts that show the top 5 APIs, Products, and Apps.

<img width="946" alt="image" src="https://user-images.githubusercontent.com/25983259/193447538-438d11cb-d05c-4266-8334-d768c21571dd.png">.

6.9 By default the Dashboards tab is displayed. To view your API event data in tabular form, select 'Discover'

![image](https://user-images.githubusercontent.com/25983259/193447878-2c8cfe72-20ed-4a9e-8b3e-b36a8318e9f5.png)

6.10 You can filter the data that is shown by clicking 'Edit', which opens the filters window:

![image](https://user-images.githubusercontent.com/25983259/193447985-a6cbae8a-cd08-4cc2-932c-05e2c72c787b.png)

6.11 You can also click Import/Export to import or export filter strings.

<img width="1159" alt="image" src="https://user-images.githubusercontent.com/25983259/193448157-22008b1d-172b-48e0-b59c-e6169070d8b2.png">

6.12 You can export the API event data from this view by clicking Actions at the upper right:

![image](https://user-images.githubusercontent.com/25983259/193448142-88bfd3a9-bec5-4a56-b5b1-dfe96f098638.png)


6.13 Besides accessing API analytics event data using Dashboard, you can also access the analytics event data by using the API Connect REST API.

6.14 API Connect REST API calls are authenticated by using a bearer token in the authorization header. Use the toolkit credentials to request a bearer token. Toolkit credentials can be found in the API Manager UI.

6.15 Go to the API Manager UI. From the home page, click the Download toolkit tile.

![image](https://user-images.githubusercontent.com/25983259/193448464-0b6e115e-7654-4a86-bb9e-1375ffdb06b7.png)

6.16 Download the credentials.
![image](https://user-images.githubusercontent.com/25983259/193448806-d3b264e6-d05b-49ce-95d1-67feda569b15.png)

6.17 Open the downloaded credentials.json file, example:

![image](https://user-images.githubusercontent.com/25983259/193448899-ac5bff02-14bd-484c-908a-c374c3a4bb22.png)

Take note of the toolkit.client_id, toolkit.client_secret, and toolkit.endpoint from this file. You will use these to get your bearer token.

6.18 Use the credentials to request a bearer token:

<img width="1108" alt="image" src="https://user-images.githubusercontent.com/25983259/193449060-9e170217-febb-4f2b-8cbe-8729db09f941.png">

curl -v -k -X POST -d '{"username": "[username]", "password": "[password]", "realm": "[realm]", "client_id": "[client_id]", "client_secret": "[client_secret]", "grant_type": "password"}' -H 'Content-Type: application/json' -H 'Accept: application/json' https://[management server platform api endpoint]/token

Where:

[username] is either the admin user or provider org owner, depending on whether you want to access cloud or provider organization scoped analytics data.

[<password] is the password for the specified <username>.

[realm] is the user registry realm for the specified <username>. By default the realm is admin/default-idp-1 for the admin user, and provider/default-idp-2 for provider organization users.

[client_id] is the client ID from the "toolkit" section of the credentials.json file, or as provided by your API Connect cloud administrator.

[client_secret] is the client secret from the "toolkit" section of the credentials.json file, or as provided by your API Connect cloud administrator.

[management server platform api endpoint] is the platform API endpoint from the "toolkit" section of the credentials.json file, or as provided by your API Connect cloud administrator.

6.19 The bearer token is returned in the access_token property:

<img width="454" alt="image" src="https://user-images.githubusercontent.com/25983259/193449032-451afcc8-f4fd-4861-99e4-a077ee04b83e.png">

Note the expires_in value, which is the number of seconds before the token expires. After expiry you need to request a new bearer token.

6.20 Use the returned bearer token to call the analytics REST API:

<img width="1101" alt="image" src="https://user-images.githubusercontent.com/25983259/193449124-7b69a306-eadd-4531-bbe4-3ce241714c20.png">

The following API call to /orgs/<provider organization>/events requires a bearer token that was requested with provider org credentials:

curl -v -k -H 'Accept: application/json' -H 'Authorization: Bearer <bearer token>' -X GET --url 'https://[management server api endpoint]/analytics/<analytics_service>/orgs/[provider organization]/events'

{
    "total": 300,
    "search_time": 3,
    "events": [...]
}

Where:

[management server api endpoint]" is the toolkit.endpoint, but with the /api at the end replaced with /analytics to access the analytics APIs.

6.21 The complete list of Analytics REST API is documented here:

https://apic-api.apiconnect.ibmcloud.com/v10/?_ga=2.162189244.1049865004.1664699487-28971200.1664599569#/IBMAPIConnectAnalyticsAPI_200/overview

![image](https://user-images.githubusercontent.com/25983259/193450136-d26e12d6-6ea9-495b-af70-67ce88b1a868.png)

Summary

You completed Lab 6 - Use Analytics REST API to retrive API event data. Throughout the tutorial, you explored the key takeaways:   

• Access API Analytics Dashboard.

• Discover API event data.

• Using Analytics REST API to query API event data 






















