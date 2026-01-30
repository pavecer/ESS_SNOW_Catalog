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

## Technical Components

### Connectors
- **ServiceNow Connection**: `pve_sharedservicenow_597ef`
  - GetCatalogs operation
  - GetCatalogCategories operation
  - GetCatalogItems operation
  - GetCatalogItem operation

### AI Builder Models
- **Adaptive Card Generation Model**: `3b1563cb-b0fb-4c7f-9e7e-080f5e5abf73`
  - Generates dynamic adaptive cards for catalogs, categories, and items
- **Item Details Model**: `61295dc5-7961-4a1a-9042-4d7e1855fb52`
  - Creates detailed view for individual catalog items

### Power Automate Flow
- **Order Processing Flow**: `f2f7fc20-6df0-f011-8406-7c1e52fd22a7`
  - Handles order submission to ServiceNow
  - Returns order number and system ID

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

### ServiceNow
- Active ServiceNow instance (configured for: `[instancename].service-now.com`)
- Service Catalog API access
- Valid connection credentials

### Microsoft Copilot Studio
- Configured ServiceNow connector
- AI Builder model access and configuration
- Power Automate flow integration

### Required Permissions
- ServiceNow catalog read permissions
- ServiceNow order creation permissions
- AI Builder model execution permissions
- Power Automate flow execution permissions

## User Experience

Users interact with the agent through a conversational interface where:
1. They are presented with adaptive cards showing available options
2. Selections are made through intuitive card interactions
3. Each step provides contextual information to guide the ordering process
4. Final confirmation includes a direct link to track the order in ServiceNow

## File Structure

```
ESS_SNOW_Catalog/
├── ESS_Topic.yml          # Main topic configuration file
└── README.md              # This documentation file
```

## ServiceNow Integration

The agent integrates with ServiceNow's Service Catalog through REST API operations:
- **Catalogs**: Retrieves top-level service catalogs
- **Categories**: Fetches categories within selected catalogs (limit: 25)
- **Items**: Gets catalog items by category (limit: 30)
- **Item Details**: Retrieves detailed information for specific items

Orders are submitted via Power Automate flow, which creates the order request in ServiceNow and returns:
- Order Number (for user reference)
- System ID (for deep linking to the request)

## Notes

- The agent uses interruption prevention during critical selection steps to ensure data integrity
- All ServiceNow data is transformed to JSON format before being passed to AI Builder models
- The confirmation card provides a direct link to the ServiceNow instance for order tracking
- User email is automatically captured from `System.User.Email` during order submission

## Future Enhancements

Potential areas for extension:
- Order history tracking
- Advanced search capabilities
- Approval workflow integration
- Multi-item ordering support
- Order modification and cancellation

---

**Model Description**: Create catalogue item based on ServiceNow available catalogs and items.
