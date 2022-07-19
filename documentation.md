# About Salsify

[Salsify](https://www.google.com/url?q=https://www.salsify.com/platform/productxm/product-information-management&sa=D&source=editors&ust=1658263055723067&usg=AOvVaw15yFXrR9hmMLg5hQLA2ElJ) was created to empower brands and retailers to grow market share in a continuously evolving digital environment. Salsify’s Product Experience Management (ProductXM) is the only true integrated solution that empowers brands with what they need to manage and syndicate product data across the digital shelf. Salsify’s Supplier Experience Management (SupplierXM) offers retailers the power  to collect, list, and enrich supplier product data with stronger collaboration and scale capabilities than ever before. These platforms help make up the Salsify CommerceXM Platform; solutions built on one interconnected platform with the only open and global network in the ecommerce industry.

The MuleSoft Certified Salsify Connector enables Brands and Manufacturers to onboard content from upstream systems to maintain a unified, continuously adaptive system of record for product content. This further streamlines processes that contribute to success on the Digital Shelf.

![](images/image44.png)

## Salsify’s Partnership with MuleSoft

Organizations who operate globally and have extensive, complex technical ecosystems leverage iPaaS tools like MuleSoft to consolidate development, deployment, maintenance, and support of all of their various integrations in one platform. By providing a certified connector in MuleSoft, Salsify enables our mutual customers to leverage best practice operations and components verified by Salsify and MuleSoft to configure an integration in a low-to-no code environment.

Key Points

-   Managing a globally distributed technology ecosystem is hard and the Digital Shelf requires execution at an accelerated rate. Successful organizations use iPaaS tools like MuleSoft to simplify and consolidate integration.
-   By providing a MuleSoft connector, Salsify allows customers to use mutually certified and validated tooling to integrate with upstream systems like their ERP, DAM, PLM, data warehouse, or other data-originating systems.
-   Integration into Salsify is now possible with reusable and configurable components in MuleSoft, rather than requiring a bespoke, fully-coded integration.

# About the connector

The connector wraps Salsify’s relevant public API endpoints and operations in Java code that is generated by MuleSoft to enable mutual customers to pull in the relevant operations and simply configure the necessary flows and test, deploy, and maintain them centrally. The Salsify Connector supports several common use cases for onboarding data into Salsify.

## Associated Use Cases

-   Expedite data onboarding in Salsify by integrating with upstream systems. Provide the necessary product information to business users quickly via sustainable, scalable integrations.
-   Create a system of record for product information. Integrate upstream systems’ data into Salsify, allowing internal resources to access and enrich product content as needed.
-   Simplify and consolidate consumer-facing images, videos, and PDFs in Salsify. Integrate your creative DAM with Salsify to provide all of your most up-to-date digital assets.
-   Enable your organization to deliver accurate content to consumers across the Digital Shelf with Salsify.

## Supported Operations

-   Authenticate easily via Salsify’s token-based authentication flow.
-   Operate on Products and Custom Records via Salsify’s publicly documented Record(s) REST endpoints[\[1\]](#ftnt1):

-   Create records via POST request to the /records/ endpoint. Supports both single and bulk actions. A bulk POST request will create up to 100 records per call.
-   Update records via PUT request to the /records/ endpoint. Supports both single (/records/{record_id}) and bulk (/records/) actions. A bulk PUT request will update up to 100 records per request.
-   Upsert records via PUT request to the /records/\_upsert endpoint. This will create new and update existing records in a single request. Both single and bulk actions are supported via this operation by passing an array of up to 100 record objects in the body of the request.

-   Operate on Digital Assets via Salsify’s publicly documented Digital Asset(s) REST endpoints:

-   Create digital assets, assign asset IDs, and populate assets’ relevant metadata via POST request to the /digital_assets/ endpoint. Supports both single and bulk actions. A bulk POST request will create up to 100 digital assets per call.
-   Update digital assets and their relevant metadata via PUT request to the /digital_assets/ endpoint. Supports both single (/digital_assets/{asset_id}) and bulk (/digital_assets/) actions. A bulk PUT request will update up to 100 digital assets per call.
-   Upsert digital assets via a PUT request to the /digital_assets/\_upsert endpoint. This will create new and update existing digital assets (as well as assign IDs and populate relevant metadata) in a single request. Both single and bulk actions are supported via this operation by passing an array of up to 100 asset objects in the body of the request.
-   Read a digital asset’s metadata via a GET request to the /digital_assets/{asset_id} endpoint to get context to inform business logic for asset linking automation or upstream DAM integration
-   Refresh digital assets that may have been updated at their source URL via a POST request to the /digital_assets/refresh endpoint. This will update the asset stored in Salsify to match the latest asset available at its source URL. Both single and bulk actions are supported via this operation by passing an array of up to 100 asset IDs in the body of the request.

-   Trigger a Salsify import via business logic defined in MuleSoft via Salsify’s publicly documented Import REST endpoint(s):

-   Trigger an import run via a POST request to the /imports/{import_id}/runs endpoint
-   Poll for the import run status programmatically, to define a flow that runs on completion or failure using a GET request to the /imports/{import_id}/runs/{import_run_id} endpoint

## Rate Limiting

In order to provide a quality experience for all users, Salsify enforces [rate limiting](https://www.google.com/url?q=https://developers.salsify.com/docs/rate-limiting-1&sa=D&source=editors&ust=1658263055729212&usg=AOvVaw0f3jRDNmfNlOWoLhjW3ifB) for API requests, including requests made via the Connector.

Rate limiting for Salsify APIs is on a per user token basis. Each token (which is tied to a user) is allowed 5,000 requests per hour, which includes all authenticated requests against our APIs, including those made in-app by the individual user's browser.

If a user account exceeds the rate limit via an automated program, the user whose API key is used for the automated activity will also not be able to access the Salsify app through the browser, so it is recommended that an integration user is created to own the integration and associated rate limit.

### Exceeded Limit

When a user exceeds this limit, they will receive a ‘429 Too Many Requests’ response. The rate limit counter will be reset at the start of each hour (e.g. if your account exceeds the rate limit at 4:45 PM, you can resume activity at 5:00 PM with a new limit of 5,000 calls).

### Bulk vs. Single Entity Operations

Bulk operations allow for updates to up to 100 entities -- Products/Records, or Digital Assets -- per request. When using bulk operations, the user will be able to make modifications up to 500,000 per hour across 5,000 requests. When using single entity operations, the user will be able to make 5,000 modifications across 5,000 requests per hour. If using both single and bulk operations in an integration, the overall request volume must stay below 5,000 requests per hour per token to avoid hitting the rate limit

# Use Cases

## General

Please note that all keys and IDs included in screenshots below are generated examples only and should not be used in your configuration.

### Authorize a Salsify Connector

For all of the following operations, the MuleSoft Developer will need an authorized connector which is able to talk to the customer’s Salsify instance. To encourage an easy to follow audit history, the developer first creates a dedicated user for the MuleSoft connector in their organization. When the developer pulls in the first Salsify operation, they create a global configuration for Salsify with their organization uuid and their dedicated user’s Auth token as shown:

![](images/image45.png)

They will be able to re-use this configuration for all flows inside one MuleSoft project.

Please be aware of Salsify’s rate limits, and know that your token in this configuration will only be able to execute 5,000 API operations per hour. As noted above, if your token exceeds this threshold in a given hour, a 429 response will be returned.

### Catch Errors from the Salsify Connector

It is possible in Anypoint studio to extend a flow to handle errors or exceptions resulting from a bad request to the Salsify API. Below is a sample flow which catches client errors and logs them to the console.

The flow is made up of a few components:

1.  An HTTP listener
2.  A Transform Message component
3.  An Update Record component
4.  An On Error Continue Error Handling flow component and a Logger
5.  The On Error Continue scope also contains a Raise Error component to cause the flow to fail.

![](images/image24.png)

#### On Error Continue

The ‘On Error Continue’ component is configured with the type of error to catch. In our case, an error from the API surfaces as a SALSIFY:CLIENT_ERROR. You can also check the ALL option from the dropdown menu.

![](images/image26.png)

#### Raise Error

The ‘Raise Error’ component can be configured to raise a custom error which in this example is named SALSIFY:API_ERROR with a description of the error. In this case, this indicates that an error response was received from the Salsify API.

![](images/image4.png)

#### The Error

For this particular example, there is a picklist property named “picklist” in the MuleSoft Developer’s organization. There exist two values for this picklist: A and B. If the MuleSoft Developer makes a request containing a picklist value other than “A” or “B”, then the request will receive an error, indicating that the value test for picklist does not exist.

Checking the Mule server runtime logs after making such a request shows that the Update Record ran into an error and that the specific error was logged:

![](images/image47.png)

The error message from the exception thrown by the Mule server can be accessed in a standard way using Dataweave. The following is the configuration of the Logger component:

![](images/image29.png)

## Products / Records

Please note that the terms ‘Products’ and ‘Records’ may be used interchangeably throughout this documentation. Salsify customers primarily store products in Salsify, but commonly also store non-product records, or Custom Records, such as parts, bundles, or brand entities. Though Products and Custom Records are distinct entities in Salsify, the Salsify API allows consolidated access to both for simplified integration via the /records/ endpoints included in connector operations. These endpoints are backwards compatible for organizations that do not leverage Custom Records in Salsify today.

### Create Non-hierarchical Product Metadata within Anypoint Studio

The MuleSoft Developer, knowing that they will be creating several different flows for each of their product types, understands the benefit of defining their metadata up front, since they can re-use metadata in any of their flows. In this example, we will use the ‘Shoes’ product type, which is not part of an inheritance hierarchy.

Since the MuleSoft Developer is planning to implement several Shoe-related flows initially, they focus on metadata for Shoe-type products. The MuleSoft Developer also knows that creating and using metadata for Shoes will be easier to start with, because the Shoes product type is not part of a hierarchy.

To accomplish this, they go into Salsify and find a Shoe product which has all of the properties on it which they know they need to map. They export this product from Salsify as JSON (via [Organization Data](https://www.google.com/url?q=https://help.salsify.com/help/download-organization-data&sa=D&source=editors&ust=1658263055734294&usg=AOvVaw39tMLrY1acirPf0oI4KP3-), [GET request to the records endpoint](https://www.google.com/url?q=https://developers.salsify.com/reference/read-record&sa=D&source=editors&ust=1658263055734648&usg=AOvVaw3FarWyjJN4yRS7-VRqZSLe), or a [Custom Channel configured to publish a filtered product/record set as JSON](https://www.google.com/url?q=https://help.salsify.com/help/channel-setup&sa=D&source=editors&ust=1658263055734856&usg=AOvVaw3uyCcbVkwSCjoERFehALux)) and pull out a specific product from the export to get the following:

<pre><code>{  
   "salsify:id": "S0000001",  
   "salsify:created_at": "2022-04-19T20:10:07.683Z",  
   "salsify:updated_at": "2022-04-19T20:10:07.683Z",  
   "salsify:version": 0,  
   "salsify:profile_asset_id": null,  
   "salsify:data_inheritance_hierarchy_level_id": "Shoe",  
   "salsify:system_id": "s-205b0d5d-a701-40ff-b054-9ea3a5b65522",  
   "Product Name": "Shoe 1",  
   "Product ID": "S0000001",  
   "Gender": "Men's",  
   "Size - US": ["8", "9","10","11", "12","13"],  
   "Width": ["D","2E"],  
   "Color": "Red",  
   "Description": "A red shoe"  
}</code></pre>

They now return to Anypoint Studio and create new Input metadata for the Salsify operation by copying the above payload, into the metadata editor:

![](images/image23.png)

They can now re-use this Shoe metadata whenever interacting with a Shoe product in Salsify in their MuleSoft Flows. MuleSoft, via the “Wrap element in a collection” option seen at the bottom left of the above Metadata editor, is able to reuse a Salsify input metadata for either single “Record” operations or Bulk “Records” operations.

### Create New Product in Salsify

The upstream system (ERP, PLM, etc.) emits an event through some medium to MuleSoft, or a flow is configured in MuleSoft to listen for events. The event contains a JSON payload of properties and their values for the product. For example:

<pre><code>{  
   "Name": "New Balance 3190",  
   "Category": "MEN/SHOES/RUNNING/NEW BALANCE 3190",  
   "ERP SKU": "M3190GR1",  
   "Color": "Grey with Red",  
   "Sizes List": ["7","7.5","8","8.5","9","9.5","10","10.5","11","11.5","12","13","14","15","16"],  
   "Widths": ["D","2E","4E"],  
   "Description": "The 3190, designed to be lightweight, is defined by its smooth ride and intelligent geometries. The REVlite midsole gives this men's running shoe a resilient bounce, and the deflection provided by the blown rubber outsole means durability you can count on. All this and the lightweight breathability of a FantomFit upper make the New Balance 3190 a perfect fit for longer training runs and race days alike."  
}</code></pre>

The MuleSoft developer knows that they need to create a new flow in their Anypoint Studio which will receive the event from the ERP, map the fields from their ERP names to their Salsify names. They’re able to leverage their ERP’s connector in MuleSoft (we use an HTTP listener to stand in for the ERP in all examples), which already knows the schema of the data coming out of ERP, so they do not need to define metadata for data coming from ERP.

The developer knows that there’s a Salsify connector for basic product operations, so they grab the connector from the Anypoint Exchange and install it into their Anypoint Studio. They determine that the “Create Records” operation is the correct choice for this flow, and they add it to their flow. At this point their flow looks like this:

![](images/image22.png)

As mentioned above, the ERP Connector (New Shoe Product Listener here) already knows the shape of the payload, as shown in the Output Metadata for the listener:

![](images/image15.png)

The developer now knows that they need to map this data to the Salsify “Create Records” operation. They know they will need to use a Transform Message component to accomplish this, but they also know that the Salsify connector does not know the shape of their schema automatically. Using the metadata defined above, they set the “Input metadata” for the “Create Records” operation to “salsify-shoes”.

With this set, they are now able to define their mappings in the Transform Message Component, as shown:

![](images/image3.png)

Their flow is now finished, and they initiate a test by manually creating a new product in their ERP. It works!

The MuleSoft Developer, being experienced in Salsify, knows that their flow does not protect against rate limits, and will error out in an unhandled way if their token hits the rate limits, but they choose not to increase the complexity of their flow at this time by handling that.

### Create Hierarchical Product Metadata in Anypoint Studio

In this example, the MuleSoft Developer needs to create a flow for a ‘Shirt’ product type, which is part of a 3 level inheritance hierarchy in Salsify (i.e. Style > Color > SKU). As such, the developer knows that they need to include the salsify:parent_id field in the metadata for shirt products.

As in the Shoes metadata example above, they export several shirt products from Salsify in order to create metadata in Anypoint Studio. This produces the following:

<pre><code>{  
   "salsify:parent_id": "Red Shirts",  
   "Product Name": "Red Button Down Shirt",  
   "Product ID": "SH00010",  
   "Description": "A specific sellable red button down",  
   "SKU": "SH00010"  
}</code></pre>

Since the MuleSoft developer is going to use this exported shirt product for metadata in a bulk operation, they create the metadata in Anypoint Studio and select “Wrap element in a collection”, as shown below:

![](images/image13.png)

### Batch Create Hierarchical Products in Salsify

Continuing from the above example with ‘Shirt’ product types, the developer’s ERP can export products at any level in this hierarchy at any time, and will include in the payload the salsify:id of the parent product. The developer knows they will need to take care to map this property appropriately at each level in their Transform Message component.

The ERP will also output a property value per product which represents the product’s level in the hierarchy.

The developer sets up the following flow, including error handling logic based on “Catching Errors from the Salsify Connector” section above.

![](images/image27.png)

They then write Dataweave directly in the transform message component, as shown below:

![](images/image7.png)

As can be seen above, this includes two important fields: salsify:parent_id, mapped from the input field “ERP Parent ID”, and salsify:data_inheritance_hierarchy_level_id, based on the input field “Hierarchy Level”.

If the developer maps these fields incorrectly, or their inputs are missing data, they will need to go to the console in Anypoint Studio to see the errors from the Salsify API.

The developer feeds this flow with the following payload, and successfully creates a new family of 3 total products:

<pre><code>[  
  {  
      "Product Name": "Red Test Shirt 1",  
      "Product ID": "SH00020",  
      "ERP Parent ID": "New Line of Red Shirts",  
      "ERP SKU": "SH00020",  
      "Hierarchy Level": "SKU"  },  
  {  
      "Product Name": "New Line of Red Shirts",  
      "Product ID": "New Line of Red Shirts",  
      "ERP Parent ID": "New Style of Shirts",  
      "Hierarchy Level": "Color"  },  
  {  
      "Product Name": "New Style of Shirts",  
      "Product ID": "New Style of Shirts",  
      "Description": "This is a new fancy line of shirts",  
      "Hierarchy Level": "Style"  }  
]</code></pre>

### Batch Modify Products in Salsify

In this example, we will be updating a set of products in Salsify and expect to see a new value in one property (not a newly created property, but newly adding a value within the property to the product record), another value of a property updated, and a final property’s value removed. Note that this same process can be followed to upsert new and updated products in Salsify by leveraging the Bulk Upsert Records operation.

![](images/image50.png)

Leveraging a Dataweave script, the payload is converted to meet the requirements of the records update endpoint. In this case, ‘Id’ is the property that these products use as the salsify:id. We’re newly adding a value to a property -- ‘Property 2’, changing the value in property ‘Belt Speed’ and clearing the value of property ‘Health’.

![](images/image48.png)

#### Payload to Mulesoft

![](images/image16.png)

#### Salsify Index Page Before Operation

![](images/image21.png)

#### Salsify Index Page After Operation

![](images/image37.png)

#### Error Example Flow

To demonstrate an error scenario, we can extend our flow to include some error handling logic

![](images/image34.png)

##### Payload to Mulesoft

This time, when making our request to Mulesoft, we’ll include a Product ID that doesn’t exist in Salsify. We see that an error message is returned originating from our Raise error component.

![](images/image28.png)

##### Error Message from Salsify

Looking through the Mulesoft logs we can identify the error that Salsify returned, informing us that the third element in our payload refers to a non-existent product.

![](images/image38.png)

The failure causes the other two products in the list not to be updated.

### Modify Single Record in Salsify

This flow is similar to the above ‘Batch Modify Products in Salsify’, but focuses on modifying a single record at a time.

![](images/image8.png)

Leveraging a Dataweave script, the developer converts our payload to meet the requirements of the single record update endpoint. We’ll be updating ‘Belt Speed’, adding a new value for ‘game’ and clearing the ‘Health’ property.

![](images/image12.png)

The connector configuration points to a single record -- ‘yellow-transport’.

![](images/image52.png)

#### Payload to Mulesoft

![](images/image18.png)

#### Product Detail Page Before Operation

![](images/image43.png)

#### Product Detail Page After Operation

#### ![](images/image42.png)Error Example Flow

To demonstrate an error scenario, we can extend our flow to include some error handling logic. In this example, we’re pointing the Update Record component to a non-existent product

![](images/image1.png)

##### Payload to Mulesoft

Using the same payload as before, we now see that the response from Mulesoft is an error message coming from the Raise error component in our error handling flow:

![](images/image36.png)

##### Error Message From Salsify

Looking in the Mulesoft runtime logs, we can see the 404 returned from Salsify:

![](images/image25.png)

## Digital Assets

### Create New or Update Asset(s) in Salsify

The originating system is configured to export digital assets that have been recently updated or created, or a MuleSoft flow has been configured to listen for new or updated assets. Its payload contains the necessary

downloadable, accessible source URL and associated custom metadata. The developer sets up the following flow:

 ![](images/image20.png)

For the upsertAssets sub-flow Transform Message component, the developer writes a Dataweave script, as shown below, to map the id, url and any associated metadata of the asset from the original payload. The two important fields to map correctly for the Bulk Upsert Digital Assets component are the ‘salsify:id’ and ‘salsify:url’.

![](images/image17.png)

For the updateProducts sub-flow Transform Message component, the developer maps their custom asset/product linking field and record/product external id appropriately. In this example, the property used on records to link assets is hardcoded as a variable in the script. However, Dataweave is a flexible language and there are many ways the target asset property could be provided, even through the initial payload from the source DAM.

![](images/image41.png)

The most important configuration of this flow are the Flow Reference components. They must be configured to forward the original Mule event to their output rather than overriding it with the result of completing their own flow. To accomplish this, the developer configures the Flow Reference ‘target’ configuration with a dummy variable name as shown below. More info about that [here](https://www.google.com/url?q=https://docs.mulesoft.com/mule-runtime/4.4/flowref-about%23considerations-when-using-a-target-variable&sa=D&source=editors&ust=1658263055751777&usg=AOvVaw1OzRVsIETOcGfIThoJMTKD).

![](images/image9.png)

The developer can then feed the following payload to the flow and expect assets to be updated or created and linked to their associated product.

![](images/image39.png)

### Create New Asset and Link to a product in Salsify

#### Common Approaches

-   Create the asset via a POST request to the /digital_assets/, with a proactively assigned ID. Upon successful POST request, pass the assigned ID as the asset ID in the desired digital asset property via a subsequent PUT request to the /records/ endpoint to update the existing product with the asset.
-   Create the asset via a POST request to the /digital_assets/, without a proactively assigned ID. Retrieve the system-generated UUID in response and pass the system-generated ID provided in the successful asset creation response as the asset ID in the desired digital asset property via a subsequent PUT request to the /records/ endpoint to update the existing product with the asset.

#### Example

When linking an asset to a record in Salsify via API, the asset must already exist with a salsify:id. The asset’s salsify:id may be assigned at creation -- this is recommended; however, if it is not assigned upon asset creation, a UUID will automatically be generated and assigned by Salsify, as long as a salsify:source_url is sent and points to a valid, publicly accessible download URL. whether proactively assigning an ID or letting the system generate it (tested the latter as well but only showing the former):

Asset creation request (POST /digital_assets/):

<pre><code>{
    "salsify:id": "my-image-123",
    "salsify:name": "my-image-123.jpg",
    "salsify:source\_url": "https://my-image-url.com/my-image-123.jpg",
    "Status": "Live"
}</code></pre>

Expected response upon asset creation:

<pre><code>{
    "salsify:id": "my-image-123",
    "salsify:source\_url": "https://my-image-url.com/my-image-123.jpg",
    "salsify:name": "my-image-123.jpg",
    "salsify:created\_at": "2022-05-05T18:45:39.411Z",
    "salsify:updated\_at": "2022-05-05T18:45:39.411Z",
    "salsify:status": "in\_progress",
    "salsify:asset\_resource\_type": "raw",
    "salsify:system\_id": "s-824d3xxx-a3b9-4b56-b354-a4a7f90c4a6e",
    "Status": "Live"
}</code></pre>

#### Important Notes

It is not possible to create an asset by directly assigning the public URL to a digital_asset data type property via a PUT request to the /records/ endpoint, because the message is processed differently than it would be via a product import. Attempting this path will result in a 422 ‘Unprocessable Entity’ error with the following response:

<pre><code>{
    "errors": [
        "Invalid reference to digital asset {url of asset}"
	    ]
}</code></pre>

If the URL used to create the asset in Salsify is not public, or if there is an issue downloading the asset, the asset may fail to upload after it is already linked to the product.

### Read Digital Asset Metadata

![](images/image35.png)

The Read Digital Asset Metadata component is configured to fetch the asset id we provide in our request to MuleSoft. Alternatively, an asset id can be hardcoded in the Digital Asset Id text box.

![](images/image6.png)

#### Payload to MuleSoft

We fetch a digital asset with id ‘001-yellow-belt’ and see MuleSoft’s response with all the metadata from digital asset.

![](images/image11.png)

#### Asset Detail Page

For reference, the following is the digital asset detail page for the digital asset id requested in the MuleSoft payload.

![](images/image10.png)

#### Error Example Flow

To demonstrate an error scenario, we can extend our flow to include some error handling logic

![](images/image46.png)

##### Payload to MuleSoft

This time, we request a digital asset that does not exist and observe that MuleSoft responds with an error originating from our Raise error component.

![](images/image14.png)

##### Error Message from Salsify

Looking at the MuleSoft runtime logs, we can see the 404 error returned from Salsify.

![](images/image51.png)

## Imports

### Listen for new file on SFTP/FTP server[\[2\]](#ftnt2) and trigger an import run

Allow a customer to initially configure a repeatable import (likely via FTP) then use MuleSoft to listen for the file to be dropped and trigger the relevant import in Salsify, check for status update, and upon ‘success’ move or delete the file.

#### High-level Steps

-   Manually upload the import file to the SFTP server (CSV, Excel, etc.) or configure the upstream system to deliver it to the SFTP as needed
-   In the Salsify application, create and map the initial import in Salsify, pointing to the FTP location of the import file
-   [Run the initial import](https://www.google.com/url?q=https://help.salsify.com/help/ftp-imports-scheduling%23setting-up-an-ftp-import&sa=D&source=editors&ust=1658263055759300&usg=AOvVaw1RPyML7Qg8KVA0kzNbMJab) and [retrieve the Import ID](https://www.google.com/url?q=https://help.salsify.com/help/trigger-an-ftp-import%23retrieve-an-import-id&sa=D&source=editors&ust=1658263055759894&usg=AOvVaw1fx3v945yE4gLVE60a19mx) from the Imports Index page or the URL
-   In the Anypoint Studio, set up the [SFTP Connector](https://www.google.com/url?q=https://docs.mulesoft.com/sftp-connector/1.4/&sa=D&source=editors&ust=1658263055760380&usg=AOvVaw2vQKircCckwMDEmoo4hqMu) (or [FTP Connector](https://www.google.com/url?q=https://docs.mulesoft.com/ftp-connector/1.5/&sa=D&source=editors&ust=1658263055760636&usg=AOvVaw2XXEqNgtcSl0TGlcFNxMRN)), which should be [connected](https://www.google.com/url?q=https://docs.mulesoft.com/ftp-connector/1.5/ftp-connection&sa=D&source=editors&ust=1658263055760891&usg=AOvVaw3m4g9GVUHbMTmF-hwWZRzx) to the relevant SFTP server / directory
-   Configure the [flow to trigger when a new or updated file is added](https://www.google.com/url?q=https://docs.mulesoft.com/ftp-connector/1.5/ftp-on-new-file&sa=D&source=editors&ust=1658263055761235&usg=AOvVaw14huDBPNZdNdcbFVfMCKyu) to the relevant directory

-   The file doesn’t need to be read in this example, because we’re solely using it’s successful upload to SFTP to trigger the import, which has already been linked to the appropriate file path.

-   Pull the Run Import operation (POST to /imports/{import_id}/runs) into the Studio flow
-   Provide any additional authentication required
-   Populate the relevant Import ID from Salsify
-   Expected response is 201 ‘Created’ with response payload containing import run metadata / status (‘running’, etc.)
-   Retrieve the import run ID from the successful response message and include it in the subsequent call(s) to get the import status
-   User pulls in the Get Import Run Status operation (GET to /imports/runs/{import_run_id})
-   Configure polling flow as outlined below to check for import state
-   Upon status returning as ‘completed’, the developer can optionally:

-   Write completion / success to log in MuleSoft
-   Pass an import completion notification to Microsoft Teams, Slack, etc.
-   Configure the flow to include [moving the file](https://www.google.com/url?q=https://docs.mulesoft.com/ftp-connector/1.5/ftp-copy-move%23configure-the-move-operation-in-studio&sa=D&source=editors&ust=1658263055762969&usg=AOvVaw2wdFx9ZuVXMPDVgzKbmfXY) to an archive subdirectory or [deleting the file](https://www.google.com/url?q=https://docs.mulesoft.com/ftp-connector/1.5/ftp-documentation%23delete&sa=D&source=editors&ust=1658263055763297&usg=AOvVaw3S8Hqz-opGxkhdVZ2-OjwZ) following successful completion
-   Configure error handling path, inclusive of updating logs and providing notification of failure

#### MuleSoft Anypoint Studio Steps

A known FTP server is set up which will host the file driving an import in Salsify. In the Salsify app, the developer or a Salsify Administrator has already set up and configured an import pointing to the previously mentioned FTP server. The developer sets up the following flow in order to trigger an import, poll it until completion and perform a finishing operation.

![](images/image32.png)

The developer will substitute the Listener component with an On New or Updated File component, which will trigger the flow once a new file is dropped into a specified directory. In order to configure the Start Import Run component, the developer will navigate to Salsify import index page and grab the id of the import they wish to associate this flow with. The developer can then configure the Start Import Run as follows:![](images/image30.png)

In order to drive the polling flow, the developer then adds a Store component (from the ObjectStore module) to store the id of the run as follows:

# ![](images/image49.png)

A Scheduler component will primarily drive the polling flow and the developer is aware that a sane polling interval should be used to avoid running against the api rate limit. In order to determine whether polling should occur, the developer adds a Retrieve component (again, from the ObjectStore module) to retrieve the id of the import run as follows:

# ![](images/image5.png)

The component will default to a known value (such as ‘N’) to prevent the flow from querying Salsify for an import run when not required. The developer also knows that the value of the retrieved key should be stored in a variable allowing the next Choice component to conditionally branch. This is configured in the Advance section of the Retrieve component. ![](images/image2.png)

The developer then sets up a Choice component. The main When scope will then be configured to run when an import has been started. It can be configured as follows:

![](images/image40.png)

The developer may do as they wish with the Default path, it will be taken when no import has been started yet or should be polled.

The developer adds a Get Import Run component in the When scope and configures the import run id to be pulled from the Mule event.

![](images/image19.png)

Again, the developer follows the Get Import Run component with a Choice component. The main scope will then be configured to run when the import has completed, whereas the Default scope does not need to perform anything (the developer may add other components to notify that the flow is still polling the import, if desired). The developer will configure the conditional in the When scope as follows:

![](images/image33.png)

Once the import is completed, the developer should make sure that the polling flow is essentially disabled by updating the Import Run Id using the Store component (from the ObjectStore module) to a known value that indicates that polling should cease.

![](images/image31.png)

With the import completed and the polling loop disabled, the developer can then follow the Store component with any finishing operations, such as removing the file from the FTP server using the Delete component from the FTP module.

The developer can now move/create a file to their FTP server (with a name and path matching the import in the Salsify app) which will trigger the MuleSoft flow to start and poll the import run. Once the import run completes, the file will then be moved/deleted from their FTP server.

* * *

[\[1\]](#ftnt_ref1) Record(s) endpoints are backwards compatible with and functionally equivalent to Product(s) endpoints.

[\[2\]](#ftnt_ref2) FTP and SFTP Connectors in MuleSoft support the same essential operations, but SFTP is generally preferred.