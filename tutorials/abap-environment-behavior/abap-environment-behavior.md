---
auto_validation: true
title: Create Behavior Definition for Managed Scenario
description: Create behavior definition and implementation for managed scenario.
primary_tag: products>sap-cloud-platform--abap-environment
tags: [  tutorial>beginner, topic>abap-development, products>sap-cloud-platform ]
time: 15
author_name: Merve Temel
author_profile: https://github.com/mervey45
---

## Prerequisites  
- You have created an SAP Cloud Platform ABAP environment trial user or
- You have created a developer user in an SAP Cloud Platform ABAP Environment system.
- You have downloaded Eclipse Photon or Oxygen and installed ABAP Development Tools (ADT). See <https://tools.hana.ondemand.com/#abap>.

## Details
### You will learn  
  - How to create behavior definition
  - How to create behavior implementation
  - How to create behavior definition for projection view

In this tutorial, wherever XXX appears, use a number (e.g. 000).

---

[ACCORDION-BEGIN [Step 1: ](Create behavior definition)]
  1. Right-click on your data definition `ZI_TRAVEL_M_XXX` and select **New Behavior Definition**. 

      ![Create behavior definition](definition.png)

  2. Check your behavior definition. Your **implementation type** is **managed**.

     Click **Next >**.

      ![Create behavior definition](definition2.png)

  3. Click **Finish** to use your transport request.

      ![Create behavior definition](definition3.png)

  4. Replace your code with following.

    ```ABAP
    managed implementation in class ZCL_BP_I_TRAVEL_M_XXX unique;

    define behavior for ZI_Travel_M_XXX alias Travel
    persistent table ztravel_xxx
    etag master last_changed_at
    lock master
    {
    // administrative fields (read only)
    field ( readonly ) last_changed_at, last_changed_by, created_at, created_by;

    // mandatory fields that are required to create a travel
    field ( mandatory ) agency_id, overall_status, booking_fee, currency_code;

    // semantic key is calculated in a determination
    field ( readonly ) travel_id;

    // standard operations for travel entity
    create;
    update;
    delete;
    }

    ```

  5. Save and activate.

      ![save and activate](activate.png)

    A warning will appear first, but after the creation of the behavior implementation it will disappear.  

    Now the **behavior definition** is created and determines the create, update and delete functionality for travel booking.


[DONE]
[ACCORDION-END]

[ACCORDION-BEGIN [Step 2: ](Create behavior definition for projection view)]
  1. Right-click on your data definition `ZC_TRAVEL_M_XXX` and select **New Behavior Definition**.

      ![Create behavior definition for projection view](projection.png)

  2. Check your behavior definition. Your implementation type is projection.

     Click **Next >**.

      ![Create behavior definition for projection view](projection2.png)

  3. Click **Finish** to use your transport request.

      ![Create behavior definition for projection view](projection3.png)

  4. Replace your code with following:

    ```ABAP
    projection;

    define behavior for ZC_TRAVEL_M_XXX alias TravelProcessor
    use etag
    {
    // scenario specific field control
    field ( mandatory ) BeginDate, EndDate, CustomerID;

    use create;
    use update;
    use delete;
    }

    ```

  5. Save and activate.

      ![save and activate](activate.png)

  6. Now switch to your service binding and double click on `TravelProcessor`.

      ![Create behavior definition for projection view](projection4.png)

  7. **Refresh** your browser and check your result.

     The create and delete button appears on the UI because of the managed scenario.
     You can create and edit travel bookings or you' re able to delete existing ones.

     Please note that the semantic key Travel ID is not calculated yet. We will do this in the next tutorial.

      ![Create behavior definition for projection view](projection5.png)

[DONE]
[ACCORDION-END]

[ACCORDION-BEGIN [Step 3: ](Test yourself)]

[VALIDATE_1]
[ACCORDION-END]
---
