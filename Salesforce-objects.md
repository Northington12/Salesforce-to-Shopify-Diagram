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
    string Id PK
    string Name
    string Email
    string Phone
    number In Store Credit
    string BillingAddress
    string ShippingAddress
    string ExternalId__c "ShopifyCustomerId"
  }

  USER {
    string Id PK
    string Name
    string Email
    boolean IsActive
  }

  RAU_PRODUCT {
    string Id PK
    string Name
    string RP_Item_Number__c "Internal SKU"
    string ProductCode "Public SKU"
    number Price__c
    number Inventory_Qty__c
    boolean Publish_to_Web__c
    string ExternalId__c "ShopifyProductId"
  }

  OPPORTUNITY {
    string Id PK
    string Name
    string AccountId FK "→ Person Account"
    string Opportunity Owner "→ User"
    percentage Opportnity Owner Split %
    2nd Sales Person "→ User"
    percentage 2nd Sales Person Split %
    3rd Sales Person "→ User"
    percentage 3rd Sales Person Split %
    string StageName
    date CloseDate
    number Account Store Credit Available
    string Initial Opportunity Source
    string Conversion Source
    
    string ExternalId__c "ShopifyOrderId"
  }

  OPPORTUNITY_PRODUCT {
    string Id PK
    string OpportunityId FK
    string Product2Id FK "→ Rau Product"
    number Quantity
    number UnitPrice
    string LineStatus__c "On Approval / Returned / Fulfilled"
    string ExternalId__c "ShopifyLineItemId"
  }

  SHIPPING_REQUEST {
    string Id PK
    string OpportunityId FK
    string Status__c "Pending/Picked/Packed/Shipped"
    string Carrier__c
    string TrackingNumber__c
    date ShipDate__c
    string ExternalId__c "ShopifyFulfillmentId"
  }

  RMA {
    string Id PK
    string OpportunityProductId FK
    number Quantity__c
    string Reason__c
    string Status__c "Requested/Approved/Received/Refunded"
    string ExternalId__c "ShopifyReturnId"
  }

  PAYMENT {
    string Id PK
    string OpportunityId FK
    number Amount__c
    string Method__c "Card/Cash/ACH/StoreCredit"
    string TransactionId__c
    string SettlementStatus__c
    string ExternalId__c "ShopifyPaymentId"
  }

  STORE_CREDIT {
    string Id PK
    string AccountId FK
    number Balance__c
    string Source__c "RMA/Manual"
    string Related_RMA__c FK
    string ExternalId__c "ShopifyStoreCreditId"
  }
