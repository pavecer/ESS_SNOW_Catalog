# ESS ServiceNow Catalog Ordering Agent

## Overview

This repository contains a Microsoft Copilot Studio topic configuration for an Employee Self Service (ESS) agent that enables users to order catalog items from ServiceNow through a conversational interface.

## Description

The ESS ServiceNow Catalog Ordering Agent provides an intelligent, guided experience for browsing and ordering items from ServiceNow's service catalog. The agent leverages AI Builder models to dynamically generate adaptive cards that present catalog information in an intuitive, user-friendly format.

## Key Features

- **Catalog Browsing**: Browse available ServiceNow catalogs through an interactive interface
- **Category Navigation**: Navigate through catalog categories to find specific items
- **Item Selection**: View and select catalog items with detailed information
- **Order Submission**: Complete order requests with automatic ServiceNow integration
- **Order Confirmation**: Receive order confirmation with direct links to track requests in ServiceNow

## Workflow

The agent follows a structured workflow to guide users through the ordering process:

1. **Retrieve Catalogs**: Fetches available catalogs from ServiceNow using connector actions
2. **Display Catalogs**: Presents catalogs through AI-generated adaptive cards
3. **Select Category**: User selects a catalog, and the agent retrieves associated categories
4. **Browse Categories**: Categories are displayed for user selection
5. **View Items**: Catalog items within the selected category are presented
6. **Item Details**: Detailed information about the selected item is shown
7. **Submit Order**: User submits the order through a Power Automate flow
8. **Confirmation**: Order number and direct link to the ServiceNow request are provided

## Solution Information

- **Solution Name**: SOLESSSNOWCatalog ([SOL] ESS SNOW Catalog)
- **Version**: 1.0.0.7
- **Publisher**: Pavel Vecer (pavelvecer)
- **Customization Prefix**: pve
- **Solution Type**: Unmanaged

## Technical Components

### Connectors
1. **ServiceNow Connection**: `pve_sharedservicenow_597ef`
   - Connection Type: shared_service-now
   - Runtime Source: Invoker
   - Operations:
     - GetCatalogs - Retrieves available service catalogs
     - GetCatalogCategories - Fetches categories within a catalog
     - GetCatalogItems - Gets catalog items by category
     - GetCatalogItem - Retrieves detailed information for a specific item
     - OrderItem - Submits order to ServiceNow

2. **Dataverse Connection**: `pve_sharedcommondataserviceforapps_5893b`
   - Connection Type: shared_commondataserviceforapps
   - Runtime Source: Embedded
   - Used for: AI Builder Custom Prompt execution

### AI Builder Models

The solution includes three AI Builder models (Root Component Type 401):

1. **Adaptive Card Generation Model**: `3b1563cb-b0fb-4c7f-9e7e-080f5e5abf73`
   - Purpose: Generates dynamic adaptive cards for catalogs, categories, and items
   - Input: ServiceNow JSON data
   - Output: Formatted adaptive card JSON

2. **Item Details Model**: `61295dc5-7961-4a1a-9042-4d7e1855fb52`
   - Purpose: Creates detailed view adaptive card for individual catalog items
   - Input: Catalog item details from ServiceNow
   - Output: Interactive item details card

3. **Input Validation Model**: `c60cf315-fd43-4eea-a15b-2df8f24a95cc`
   - Purpose: Custom prompt model to compare and validate required inputs
   - Used in: Order processing flow to ensure all required fields are provided
   - Input: Catalog item variables and user-submitted data
   - Output: Validation result

### Power Automate Flow

**Flow Name**: ESS EXAMPLE ServiceNow Order Item  
**Flow ID**: `f2f7fc20-6df0-f011-8406-7c1e52fd22a7` (Root Component Type 29)

**Flow Actions:**
1. **Trigger**: Manual (Skills trigger)
   - Receives SNOWItemDetails (JSON string with order data)
   - Receives UserName (email address)

2. **Get Catalog Item**: Retrieves catalog item details from ServiceNow
   - Uses sys_id from submitted order data

3. **Compare the Needed Input**: AI Builder Custom Prompt
   - Validates that submitted data matches required catalog item variables
   - Ensures all mandatory fields are filled

4. **Order Item**: Submits order to ServiceNow
   - Parameters:
     - `sys_id`: Catalog item system ID
     - `sysparm_quantity`: Quantity from user input
     - `sysparm_requested_for`: User email address

5. **Respond to the Agent**: Returns order confirmation
   - `snowordernumber`: ServiceNow request number
   - `snowordersysid`: ServiceNow request system ID

### Variables
- `Topic.GetCatalogs`: Stores catalog data from ServiceNow
- `Topic.acSnowCatalog`: AI-generated adaptive card for catalogs
- `Topic.recSnowCatalog`: Parsed user selection from catalog card
- `Topic.GetCatalogCategories`: Stores category data
- `Topic.varCategoriesAC`: Adaptive card for categories
- `Topic.recSnowCategory`: Parsed category selection
- `Topic.GetCatalogItems`: Stores catalog items
- `Topic.varItemsAC`: Adaptive card for items
- `Topic.recSnowItem`: Parsed item selection
- `Topic.GetCatalogItem`: Detailed item information
- `Topic.varItemAC`: Adaptive card for item details
- `Global.varOrderItemDetails`: Order details for submission
- `Global.SNOWOrderNumber`: Generated ServiceNow order number
- `Global.SNOWOrderSysID`: ServiceNow system ID for the order

## Configuration Requirements

### Prerequisites
- Microsoft Copilot Studio license
- Power Platform environment
- ServiceNow instance with Service Catalog enabled
- AI Builder capacity for custom models and prompts

### ServiceNow Configuration
- Active ServiceNow instance (example: `dev310193.service-now.com`)
- Service Catalog API access enabled
- Valid connection credentials (OAuth or Basic Auth)
- Required ServiceNow tables:
  - `sc_catalog` - Service catalogs
  - `sc_cat_item_category` - Catalog categories
  - `sc_cat_item` - Catalog items
  - `sc_request` - Service requests

### Microsoft Copilot Studio Configuration
- Import the SOLESSSNOWCatalog solution
- Configure ServiceNow connector with valid credentials
- Ensure AI Builder models are properly trained and published
- Configure Power Automate flow connections

### Connection References
Update the following connection references after import:
1. `pve_sharedservicenow_597ef` - ServiceNow connector
2. `pve_sharedcommondataserviceforapps_5893b` - Dataverse connector

### Required Permissions
- **ServiceNow**:
  - `sc_catalog.read` - Read access to catalogs
  - `sc_cat_item_category.read` - Read access to categories
  - `sc_cat_item.read` - Read access to catalog items
  - `sc_request.create` - Create service requests
- **Power Platform**:
  - Environment Maker role (minimum)
  - AI Builder model execution permissions
  - Flow execution permissions
- **Copilot Studio**:
  - Bot author or Bot contributor role

## User Experience

Users interact with the agent through a conversational interface where:
1. They are presented with adaptive cards showing available options
2. Selections are made through intuitive card interactions
3. Each step provides contextual information to guide the ordering process
4. Final confirmation includes a direct link to track the order in ServiceNow

## File Structure

```
ESS_SNOW_Catalog/
├── ESS_Topic.yml                                    # Main Copilot Studio topic configuration
├── README.md                                        # This documentation file
├── SOLESSSNOWCatalog_1_0_0_7.zip                   # Packaged solution file
└── SOLESSSNOWCatalog_1_0_0_7/                      # Unpacked solution directory
    ├── [Content_Types].xml                          # Content type definitions
    ├── solution.xml                                 # Solution manifest with metadata
    ├── customizations.xml                           # Solution customizations
    └── Workflows/                                   # Power Automate flows
        └── ESSEXAMPLEServiceNowOrderItem-F2F7FC20-6DF0-F011-8406-7C1E52FD22A7.json
```

## Solution Components Summary

### Root Components (from solution.xml)
The solution includes the following components:

| Component Type | ID | Description |
|----------------|-----|-------------|
| 29 (Workflow) | f2f7fc20-6df0-f011-8406-7c1e52fd22a7 | ESS EXAMPLE ServiceNow Order Item flow |
| 401 (AI Model) | 3b1563cb-b0fb-4c7f-9e7e-080f5e5abf73 | Adaptive Card Generation Model |
| 401 (AI Model) | 61295dc5-7961-4a1a-9042-4d7e1855fb52 | Item Details Model |
| 401 (AI Model) | c60cf315-fd43-4eea-a15b-2df8f24a95cc | Input Validation Model |

## ServiceNow Integration

The agent integrates with ServiceNow's Service Catalog through REST API operations:

### API Operations
1. **GetCatalogs**: Retrieves top-level service catalogs
   - Endpoint: `/api/sn_sc/servicecatalog/catalogs`
   - Returns: List of available catalogs

2. **GetCatalogCategories**: Fetches categories within selected catalogs
   - Endpoint: `/api/sn_sc/servicecatalog/catalogs/{catalogId}/categories`
   - Parameter: `catalogId`, `sysparm_limit=25`
   - Returns: Categories belonging to the catalog

3. **GetCatalogItems**: Gets catalog items by category
   - Endpoint: `/api/sn_sc/servicecatalog/items`
   - Parameter: `sysparm_category`, `sysparm_limit=30`
   - Returns: Catalog items in the category

4. **GetCatalogItem**: Retrieves detailed information for specific items
   - Endpoint: `/api/sn_sc/servicecatalog/items/{sys_id}`
   - Parameter: `sys_id`
   - Returns: Complete item details including variables

5. **OrderItem**: Submits catalog item order
   - Endpoint: `/api/sn_sc/servicecatalog/items/{sys_id}/order_now`
   - Parameters:
     - `sys_id`: Item system ID
     - `sysparm_quantity`: Order quantity
     - `sysparm_requested_for`: Requestor email
   - Returns: Request number and system ID

### Order Processing Flow
The Power Automate flow validates and processes orders:
1. Receives order data from Copilot Studio topic
2. Retrieves catalog item to get required variables
3. Uses AI Builder to validate submitted data against requirements
4. Submits order to ServiceNow
5. Returns confirmation with:
   - **Order Number** (e.g., `REQ0010001`) - for user reference
   - **System ID** - for deep linking to the request

### Deep Linking
Order confirmations include direct links to ServiceNow:
```
https://dev310193.service-now.com/now/nav/ui/classic/params/target/sc_request.do?sys_id={SNOWOrderSysID}
```

## Installation and Deployment

### Step 1: Import Solution
1. Download the `SOLESSSNOWCatalog_1_0_0_7.zip` file
2. Navigate to [Power Apps](https://make.powerapps.com)
3. Select your target environment
4. Go to **Solutions** > **Import solution**
5. Browse and select the ZIP file
6. Click **Next** and follow the import wizard

### Step 2: Configure Connections
After import, configure the connection references:
1. Open the **SOLESSSNOWCatalog** solution
2. Navigate to **Connection References**
3. For `pve_sharedservicenow_597ef`:
   - Create or select ServiceNow connection
   - Provide instance URL, username, and password/OAuth token
4. For `pve_sharedcommondataserviceforapps_5893b`:
   - Create or select Dataverse connection
   - Authenticate with appropriate credentials

### Step 3: Verify AI Builder Models
1. In the solution, locate the three AI Builder models
2. Ensure each model is published and ready
3. Test models with sample data if needed

### Step 4: Configure Power Automate Flow
1. Open the **ESS EXAMPLE ServiceNow Order Item** flow
2. Verify all connections are properly configured
3. Test the flow with sample JSON data:
   ```json
   {
     "sys_id": "sample_id",
     "quantity": "1",
     "Additional_software_requirements": "test"
   }
   ```
4. Save and turn on the flow

### Step 5: Import Copilot Studio Topic
1. Open Microsoft Copilot Studio
2. Navigate to your target agent
3. Go to **Topics** > **Add topic** > **From YAML**
4. Upload or paste the contents of `ESS_Topic.yml`
5. Update connection references in the topic if needed
6. Save and test the topic

### Step 6: Test End-to-End
1. Open your Copilot agent
2. Trigger the ESS catalog ordering topic
3. Walk through the complete flow:
   - Select a catalog
   - Choose a category
   - Pick an item
   - Fill in required fields
   - Submit the order
4. Verify order creation in ServiceNow

## Notes

- The agent uses interruption prevention during critical selection steps to ensure data integrity
- All ServiceNow data is transformed to JSON format before being passed to AI Builder models
- The confirmation card provides a direct link to the ServiceNow instance for order tracking
- User email is automatically captured from `System.User.Email` during order submission

## Future Enhancements

Potential areas for extension:
- Order history tracking and status updates
- Advanced search capabilities with filtering
- Approval workflow integration for high-value items
- Multi-item ordering (shopping cart functionality)
- Order modification and cancellation capabilities
- Favorites and frequently ordered items
- Cost center and budget validation
- Manager approval routing
- Email notifications for order status changes

## Troubleshooting

### Common Issues

**Connection Failures**
- Verify ServiceNow instance URL is correct
- Check connection credentials are valid
- Ensure ServiceNow API is accessible from Power Platform
- Verify firewall rules allow outbound connections

**AI Builder Model Errors**
- Ensure models are published and not in draft state
- Check AI Builder capacity/credits are available
- Verify model inputs match expected schema

**Flow Execution Failures**
- Review flow run history for specific error messages
- Check that all connections are authenticated
- Verify ServiceNow user has required permissions
- Ensure catalog items have proper configuration

**Order Submission Issues**
- Validate required variables are being captured
- Check quantity field is numeric
- Verify user email format is correct
- Ensure catalog item is active and orderable

## Support and Contribution

For issues, questions, or contributions:
- Repository: [ESS_SNOW_Catalog](https://github.com/pavecer/ESS_SNOW_Catalog)
- Publisher: Pavel Vecer

## License

This solution is provided as-is for demonstration and educational purposes.

---

**Model Description**: Create catalogue item based on ServiceNow available catalogs and items.
