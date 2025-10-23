```mermaid
erDiagram
  PERSON_ACCOUNT ||--o{ OPPORTUNITY : "creates/owns"
  USER ||--o{ OPPORTUNITY : "owner"
  RAU_PRODUCT ||--o{ OPPORTUNITY_PRODUCT : "referenced by"
  OPPORTUNITY ||--o{ OPPORTUNITY_PRODUCT : "contains"
  OPPORTUNITY ||--o{ SHIPPING_REQUEST : "fulfillment for stage in {On Approval, CwC, Closed Won}"
  OPPORTUNITY_PRODUCT ||--o{ RMA : "returns for"
  OPPORTUNITY ||--o{ PAYMENT : "paid by"
  PERSON_ACCOUNT ||--o{ STORE_CREDIT : "credit ledger"

  PERSON_ACCOUNT {
    Id PK
    Name
    Email
    Phone
    BillingAddress
    ShippingAddress
    ExternalId__c "ShopifyCustomerId"
  }

  USER {
    Id PK
    Name
    Email
    IsActive
  }

  RAU_PRODUCT {
    Id PK
    Name
    RP_Item_Number__c "Internal SKU"
    ProductCode "Public SKU"
    Price__c
    Inventory_Qty__c
    Publish_to_Web__c
    ExternalId__c "ShopifyProductId"
  }

  OPPORTUNITY {
    Id PK
    Name
    AccountId FK "→ Person Account"
    OwnerId FK "→ User"
    StageName
    CloseDate
    Amount
    CurrencyIsoCode
    ExternalId__c "ShopifyOrderId"
  }

  OPPORTUNITY_PRODUCT {
    Id PK
    OpportunityId FK
    Product2Id FK "→ Rau Product"
    Quantity
    UnitPrice
    LineStatus__c "On Approval / Returned / Fulfilled"
    ExternalId__c "ShopifyLineItemId"
  }

  SHIPPING_REQUEST {
    Id PK
    OpportunityId FK
    Status__c "Pending/Picked/Packed/Shipped"
    Carrier__c
    TrackingNumber__c
    ShipDate__c
    ExternalId__c "ShopifyFulfillmentId"
  }

  RMA {
    Id PK
    OpportunityProductId FK
    Quantity__c
    Reason__c
    Status__c "Requested/Approved/Received/Refunded"
    ExternalId__c "ShopifyReturnId"
  }

  PAYMENT {
    Id PK
    OpportunityId FK
    Amount__c
    Method__c "Card/Cash/ACH/StoreCredit"
    TransactionId__c
    SettlementStatus__c
    ExternalId__c "ShopifyPaymentId"
  }

  STORE_CREDIT {
    Id PK
    AccountId FK
    Balance__c
    Source__c "RMA/Manual"
    Related_RMA__c FK
    ExternalId__c "ShopifyStoreCreditId"
  }
```
