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
    number In_Store_Credit
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
    string Opportunity Name
    string Account Name FK "PersonAccount"
    string OpportunityOwnerId FK "User"
    number OpportunityOwnerSplitPercent
    string Sales_Person_2_Id FK "User"
    number Sales_Person_2_SplitPercent
    string Sales_Person_3_Id FK "User"
    number Sales_Person_3_SplitPercent
    string StageName
    date CloseDate
    number Account_Store_Credit_Available
    string Initial_Opportunity_Source
    string Conversion_Source
    string ExternalId__c "ShopifyOrderId"
    string IOS Split
    string IOS Net Sale
    number Total Sale
    number Shipping Charged Total
    number Total Estimated Tax
    number Sales Tax
    number Net Sale
    number Total Payment
    number Balance Due
    string Ship to Attention
    number Ship to Phone
    string Ship to Address 1
    string Ship to Address 2
    string Ship to City
    string Ship to State
    string Ship to State
    string Ship to Zip
    string Ship to Country
    string Payment Terms
    string Invoice Comment
    boolean Opp Commit Tax
    number Number of Successful Commits
    number Number of Failed Commits
    date Estimate Tax Date
    date Commit Tax Date
    string Sales Tax Notes and Comments
  }

  OPPORTUNITY_PRODUCT {
    string Id PK
    string OpportunityId FK
    string Product2Id FK "RauProduct"
    number Quantity
    number UnitPrice
    string LineStatus__c "On Approval / Returned / Fulfilled"
    string ExternalId__c "ShopifyLineItemId"
  }

  SHIPPING_REQUEST {
    string Shipping Request Number PK
    string Request Status
    string Short Description
    string Sales Person
    string Opportunity Stage
    string Opportunity Prod
    string Account
    string Shipping Request Type
    boolean Okay to Ship
    boolean Paid in Full
    string Shipping When?
    string Rau Product
    string Opportunity FK
    string Status__c "Pending/Picked/Packed/Shipped"
    string Carrier__c
    string TrackingNumber__c
    date ShipDate__c
    string ExternalId__c "ShopifyFulfillmentId"
    string Ship to Attention
    number Ship to Phone
    string Ship to Address 1
    string Ship to Address 2
    string Ship to City
    string Ship to State
    string Ship to State
    string Ship to Zip
    string Ship to Country
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

```
