# Cisco Umbrella Data Connector for Microsoft Sentinel

Azure Function App that ingests Cisco Umbrella logs from AWS S3 and sends them to Microsoft Sentinel (Log Analytics).

## Supported Log Types

- DNS logs
- Proxy logs
- IP logs
- Cloud Firewall logs
- Firewall logs
- DLP logs
- RAVPN logs
- Audit logs
- ZTNA logs
- Intrusion logs
- ZTA Flow logs
- File Event logs

## Deploy to Azure

### Option 1: One-Click Deploy

[![Deploy to Azure](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FSpen789%2Fcisco-umbrella-connector%2Fmain%2Fazuredeploy.json)

### Option 2: Custom Template (Copy & Paste)

1. Go to the [Azure Portal](https://portal.azure.com)
2. Search for **Deploy a custom template**
3. Click **Build your own template in the editor**
4. Delete everything in the editor
5. Copy the contents of [azuredeploy.json](https://raw.githubusercontent.com/Spen789/cisco-umbrella-connector/main/azuredeploy.json) and paste it in
6. Click **Save**
7. Fill in the required parameters:

| Parameter | Description |
|-----------|-------------|
| **FunctionName** | Name for the Function App (max 11 characters) |
| **WorkspaceID** | Microsoft Sentinel Log Analytics Workspace ID |
| **WorkspaceKey** | Microsoft Sentinel Log Analytics Primary Key |
| **S3Bucket** | Cisco Umbrella S3 bucket path (e.g. `mybucket/my-org-id`) |
| **AWSAccessKeyId** | AWS Access Key ID with S3 read permissions |
| **AWSSecretAccessKey** | AWS Secret Access Key |

8. Select your **Resource Group** and **Region**
9. Click **Review + Create**, then **Create**

The function will start running automatically every 10 minutes, pulling logs from your Cisco Umbrella S3 bucket and sending them to Sentinel.

## Memory Optimisations

This connector includes fixes for the out-of-memory (exit code 137) issue on Consumption plan:

- Reduced event queue size from 10,000 to 2,000 per connector
- Reduced bulk batch count from 10 to 3
- Memory is freed after each file is processed
- Connectors are flushed after each file instead of accumulating

## Prerequisites

- Microsoft Sentinel workspace
- Cisco Umbrella with S3 log export enabled
- AWS IAM credentials with read access to the Umbrella S3 bucket
