# SIEM Visualization Dashboards

This repository contains detailed instructions for creating SIEM (Security Information and Event Management) visualizations using the Elastic Stack (ELK Stack). These visualizations are designed to monitor and analyze various security-related events within a network environment.

## Table of Contents

- [Overview](#overview)
- [Prerequisites](#prerequisites)
- [Visualizations](#visualizations)
  - [Failed Logon Attempts (All Users)](#failed-logon-attempts-all-users)
  - [Failed Logon Attempts (Disabled Users)](#failed-logon-attempts-disabled-users)
  - [Successful RDP Logon Related to Service Accounts](#successful-rdp-logon-related-to-service-accounts)
  - [Users Added or Removed from a Local Group](#users-added-or-removed-from-a-local-group)
- [Usage](#usage)
- [Contributing](#contributing)
- [License](#license)

## Overview

This repository provides examples of SIEM visualizations focusing on common security events like failed logon attempts, successful Remote Desktop Protocol (RDP) logons, and user management activities. These visualizations help in monitoring and detecting potential security incidents.

## Prerequisites

Before you can create these visualizations, ensure you have the following:
- Access to an Elastic Stack (ELK Stack) deployment.
- Log data indexed in Elasticsearch, including Windows event logs.
- Basic knowledge of KQL (Kibana Query Language).

## Visualizations

### Failed Logon Attempts (All Users)

This visualization captures all failed logon attempts across all users.

**Steps:**
1. Navigate to the Kibana Dashboard and click on "Create new dashboard".
2. Set the time filter to "Last 15 years".
3. Apply a filter for Event ID `4625` to capture failed logon attempts.

![image](https://github.com/user-attachments/assets/c1f04d10-806c-41b5-93bf-2d617f23b2a1)

4. Set the index pattern to `windows*`.

![image](https://github.com/user-attachments/assets/2e47bdef-000d-41c4-9c33-a9cf69ce7a66)


5. Create a Table visualization with the following fields:
   - `user.name.keyword` (Username)

      ![image](https://github.com/user-attachments/assets/d43085da-19a6-4d51-ac62-d049c9958b24)


   - `host.hostname.keyword` (Hostname)

      ![image](https://github.com/user-attachments/assets/5d737fe3-01bf-4f9a-a01d-cc79dbf6c717)

   - Under metrics use `Count` 

      ![image](https://github.com/user-attachments/assets/4cb5e48d-45e8-49ab-b242-c5b97c787a1e)
     
   - `winlog.logon.type,keyword` (Logon Type)

      ![image](https://github.com/user-attachments/assets/b2e549d9-c393-4c0c-9071-3b6e2cebf2f3)



6. Add the following filters:
   - Exclude specific machine accounts like `DESKTOP-DPOESND`, `WIN-OK9BH1BCKSD`, `WIN-RMMGJA7T9TC`.
   - Exclude computer accounts using the KQL query `NOT user.name: *$ AND winlog.channel.keyword: Security`.

      

7. Save the visualization and add it to the dashboard.

   ![image](https://github.com/user-attachments/assets/37e6b53b-8eb3-4fb5-a30d-bfa48d115490)

### Failed Logon Attempts (Disabled Users)

This visualization focuses on failed logon attempts made by disabled user accounts.

**Steps:**
1. Create a new visualization in the Kibana Dashboard.
2. Set the time range appropriately.
3. Apply a filter for Event ID `4625`.
4. Use the filter `winlog.event_data.SubStatus: "0xC0000072"` to isolate attempts by disabled users.

  ![image](https://github.com/user-attachments/assets/49ccba2b-7ea9-4795-b91f-894ed6715a9f)


5. Create a Table visualization with the following fields:
   - `user.name.keyword` (Username)
   - `host.hostname.keyword` (Hostname)
6. Save the visualization and add it to the dashboard.

  ![image](https://github.com/user-attachments/assets/e6382405-759c-4d0f-90d3-9ad12e8c3330)


### Successful RDP Logon Related to Service Accounts

This visualization monitors successful RDP logons performed by service accounts.

**Steps:**
1. Create a new visualization in the Kibana Dashboard.
2. Apply a filter for Event ID `4624`.

  ![image](https://github.com/user-attachments/assets/d86f1994-8373-4e95-8022-b6830501bbf8)


3. Add a logon type filter `winlog.logon.type` to focus on RemoteInteractive (RDP) logons.

  ![image](https://github.com/user-attachments/assets/9ac72f9c-3af4-4289-ba63-1c6307d68995)

4. Create a Table visualization with the following fields:
   - `user.name.keyword` (Service Account Username)

      ![image](https://github.com/user-attachments/assets/7a00ead2-b350-446c-b0fc-0712d980fdaf)

   - Under metrics use `Count` 

      ![image](https://github.com/user-attachments/assets/4cb5e48d-45e8-49ab-b242-c5b97c787a1e)
     
   - `host.hostname.keyword` (Hostname)

     ![image](https://github.com/user-attachments/assets/0dbfbf68-ef43-43a9-8a44-e3052fdda6b8)
 

   - `related.ip.keyword` (IP Address)

      ![image](https://github.com/user-attachments/assets/19579a5c-8cc3-4578-883a-33bcb73f4943)
      

5. Filter for service accounts using `user.name: "svc-*"`.

  ![image](https://github.com/user-attachments/assets/9cc71968-0dc4-478f-b000-258fe08bde02)

6. Save the visualization and add it to the dashboard.

  ![image](https://github.com/user-attachments/assets/efa4c952-b140-43fc-80af-d3d7f48babe0)


### Users Added or Removed from a Local Group

This visualization tracks users added or removed from a local group, focusing on the "Administrators" group.

**Steps:**
1. Create a new visualization in the Kibana Dashboard.
2. Apply filters for Event IDs `4732` (user added to group) and `4733` (user removed from group).
3. Focus on the "Administrators" group.

  ![image](https://github.com/user-attachments/assets/494d3143-8963-4271-9973-276e7d33d33e)

4. Create a Table visualization with the following fields:
   - `user.name.keyword` (Username)

      ![image](https://github.com/user-attachments/assets/a483c91b-e23e-42ac-9854-bc4a3cd766d5)

   - `winlog.event_data.MemberSid.keyword`

      ![image](https://github.com/user-attachments/assets/f3695c0b-c7b7-4181-9a92-deac1a84ca1b)

   - `group.name.keyword`

      ![image](https://github.com/user-attachments/assets/847fb790-22d2-407c-9f44-dc7babb15ce4)
     
   - `event.action.keyword`

      ![image](https://github.com/user-attachments/assets/fa4d843d-6939-4da6-b75d-0c8f2bab4f6b)

   - `host.hostname.keyword` (Hostname)

      ![image](https://github.com/user-attachments/assets/c1091142-e922-4068-80ec-d691643efd94)

5. Set the date range from March 5th, 2023, to the current date.
6. Save the visualization and add it to the dashboard.
  
  ![image](https://github.com/user-attachments/assets/d7a269f8-649d-4109-96c9-ef1a471b4aa3)

## Usage

To use these visualizations:
1. Follow the step-by-step instructions for each example to set up the visualizations in your Kibana Dashboard.
2. Customize the time range and filters as per your environment and requirements.
3. Monitor the dashboards regularly to detect and respond to security events.
