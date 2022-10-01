# ASEANZK-CP4I-Practicum-apic
CP4I API Connect enablement 
### Tutorial Overview
<img width="698" alt="image" src="https://user-images.githubusercontent.com/25983259/193075472-7df79cae-05e1-49bb-946c-258aaab1e55f.png">

### Tutorial Use Case:

A regional bank wants to move to the cloud and adopt a platform to share data among registered partners but is concerned about continuing to meet regulatory compliance standards and mitigating potential security risks. As their integration specialist, you have been asked to demonstrate that access to account information via APIs all is secure no matter where they reside, and that the sensitive data could be protected. In this example, you’ll use security-rich features within the API management lifecycle to help proxy an API and mask sensitive data from the response, using built-in policies
and mitigating potential security risks.

In this tutorial, you will be using the API Connect tooling to design and meet the following business requirement.

  - Bank needs to provide customer account information to digital channels. e.g Fintech

  - Create an API to allow digital channel developer to access & test the API from Bank’s developer portal.

  - Customer account information is stored in core banking system with the virtual API endpoint: 
(https://cp4i-mock-api.mybluemix.net/accounts)

  - Sensitive data such as Identity Card No. & Phone No. must be masked before sending response back to the consumer app.

  - Define API Plan with the quota,  e.g. 100 calls per minute

  - Only the authorized consumer app can consume the API with valid Client ID key.
  
  
### Lab Summary:
  
- Lab 1: Bank developer persona - Create API for external channel to access

- Lab 2: Bank admin persona -  Define API Plan and Publish API

- Lab 3: Consumer organisation developer persona - Self Service on-boarding, subscribe and consume APIs


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

3.24 Click the product such as "Customer-xxx" then select the "Account-xxx" API from the list.

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


#### 4: Secure API










