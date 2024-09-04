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

   - `winlog.logon.type,keyword` (Logon Type)

![image](https://github.com/user-attachments/assets/b2e549d9-c393-4c0c-9071-3b6e2cebf2f3)

   - Under metrics use `Count` 

![image](https://github.com/user-attachments/assets/4cb5e48d-45e8-49ab-b242-c5b97c787a1e)

6. Add the following filters:
   - Exclude specific machine accounts like `DESKTOP-DPOESND`, `WIN-OK9BH1BCKSD`, `WIN-RMMGJA7T9TC`.
   - Exclude computer accounts using the KQL query `NOT user.name: *$ AND winlog.channel.keyword: Security`.

![image](https://github.com/user-attachments/assets/37e6b53b-8eb3-4fb5-a30d-bfa48d115490)

7. Save the visualization and add it to the dashboard.

### Failed Logon Attempts (Disabled Users)

This visualization focuses on failed logon attempts made by disabled user accounts.

**Steps:**
1. Create a new visualization in the Kibana Dashboard.
2. Set the time range appropriately.
3. Apply a filter for Event ID `4625`.
4. Use the filter `winlog.event_data.SubStatus: "0xC0000072"` to isolate attempts by disabled users.
5. Create a Table visualization with the following fields:
   - `user.name.keyword` (Username)
   - `host.hostname.keyword` (Hostname)
6. Save the visualization and add it to the dashboard.

### Successful RDP Logon Related to Service Accounts

This visualization monitors successful RDP logons performed by service accounts.

**Steps:**
1. Create a new visualization in the Kibana Dashboard.
2. Apply a filter for Event ID `4624`.
3. Add a logon type filter `winlog.logon.type: "10"` to focus on RemoteInteractive (RDP) logons.
4. Filter for service accounts using `user.name: "svc-*"`.
5. Create a Table visualization with the following fields:
   - `user.name.keyword` (Service Account Username)
   - `host.hostname.keyword` (Hostname)
   - `related.ip.keyword` (IP Address)
6. Save the visualization and add it to the dashboard.

### Users Added or Removed from a Local Group

This visualization tracks users added or removed from a local group, focusing on the "Administrators" group.

**Steps:**
1. Create a new visualization in the Kibana Dashboard.
2. Apply filters for Event IDs `4732` (user added to group) and `4733` (user removed from group).
3. Set the date range from March 5th, 2023, to the current date.
4. Focus on the "Administrators" group.
5. Create a Table visualization with the following fields:
   - `user.name.keyword` (Username)
   - `host.hostname.keyword` (Hostname)
6. Save the visualization and add it to the dashboard.

## Usage

To use these visualizations:
1. Follow the step-by-step instructions for each example to set up the visualizations in your Kibana Dashboard.
2. Customize the time range and filters as per your environment and requirements.
3. Monitor the dashboards regularly to detect and respond to security events.
