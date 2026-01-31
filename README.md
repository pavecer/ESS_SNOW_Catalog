# ESS ServiceNow Catalog Ordering Agent

## Overview

Microsoft Copilot Studio agent for Employee Self Service (ESS) that enables users to order ServiceNow catalog items through a conversational interface with AI-generated adaptive cards.

**Version**: 1.0.0.8 | **Publisher**: Pavel Vecer

## Agent Flow

The ESS agent guides users through a 5-step ordering process:

1. **Select Catalog** → Displays available ServiceNow catalogs
2. **Choose Category** → Shows categories within the selected catalog
3. **Pick Item** → Lists catalog items in the category
4. **Review Details** → Presents item details with order form
5. **Confirm Order** → Submits to ServiceNow and returns order number with tracking link

### AI Builder Prompts

The agent uses three AI Builder models to generate dynamic user interfaces:

1. **Adaptive Card Generator** (`3b1563cb-b0fb-4c7f-9e7e-080f5e5abf73`)
   - Converts ServiceNow catalog data into interactive adaptive cards
   - Used for: Catalogs, Categories, and Items display

2. **Item Details Generator** (`61295dc5-7961-4a1a-9042-4d7e1855fb52`)
   - Creates detailed item view with order form
   - Includes all required fields from ServiceNow variables

## Power Automate Flow

**Flow**: ESS EXAMPLE ServiceNow Order Item (`f2f7fc20-6df0-f011-8406-7c1e52fd22a7`)

The flow handles order creation with the following steps:

```
Trigger (from Copilot) 
  ↓ Receives: Order data JSON + User email
Get Catalog Item 
  ↓ Fetches: Item details from ServiceNow
Extract & Map Variables 
  ↓ Processes: Item variables and maps to order format
  ↓ Handles: Checkboxes, text fields, and nested structures
Submit Order 
  ↓ Creates: ServiceNow request with quantity and requestor
Return Response 
  ↓ Returns: Order number + System ID
```

**Output**: 
- `snowordernumber`: ServiceNow request number (e.g., REQ0010001)
- `snowordersysid`: System ID for deep linking to request

## Quick Start

### Installation
1. Import `SOLESSSNOWCatalog_1_0_0_8.zip` into Power Platform
2. Configure ServiceNow connection (`pve_sharedservicenow_597ef`)
3. Verify AI Builder models are published
4. Turn on the Power Automate flow
5. Import `ESS_Topic.yml` into Copilot Studio

### Prerequisites
- Microsoft Copilot Studio license
- ServiceNow instance with Service Catalog API
- AI Builder capacity

## Repository Contents

```
ESS_SNOW_Catalog/
├── ESS_Topic.yml                          # Copilot Studio topic
├── SOLESSSNOWCatalog_1_0_0_8.zip         # Power Platform solution
└── SOLESSSNOWCatalog_1_0_0_8/            # Unpacked solution
    └── Workflows/
        └── ESSEXAMPLEServiceNowOrderItem-F2F7FC20-6DF0-F011-8406-7C1E52FD22A7.json
```

---

**Solution**: SOLESSSNOWCatalog | **Type**: Unmanaged | **Prefix**: pve
