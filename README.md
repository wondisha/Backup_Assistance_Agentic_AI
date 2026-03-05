# Snowflake Backup Assistant Agent

A Cortex Agent for managing Snowflake database, schema, and table backups through natural language conversations.

## Overview

The Backup Assistant Agent provides an AI-powered interface for managing Snowflake's native backup capabilities. Users can create backup sets, configure automated backup policies, restore from backups, and manage legal holds—all through conversational interactions.

## Features

- **Multi-level Backups**: Support for table, schema, and database-level backups
- **Automated Scheduling**: Configure hourly, daily, or weekly backup policies
- **Flexible Retention**: Set custom expiration periods for automatic cleanup
- **Disaster Recovery**: Restore objects from any point-in-time backup
- **Legal Holds**: Compliance support to prevent backup deletion during litigation
- **Policy Management**: Suspend/resume backup creation and expiration independently

## Prerequisites

- Snowflake account with Cortex Agents enabled
- `ACCOUNTADMIN` role (or equivalent privileges)
- Active warehouse (default: `COMPUTE_WH`)

## Installation

### Quick Start

```sql
-- Run the complete setup script
USE ROLE ACCOUNTADMIN;
-- Execute setup/backup_assistant_agent.sql
┌─────────────────────────────────────────────────────────────┐
│                    BACKUP_ASSISTANT Agent                    │
│                    (claude-4-sonnet)                         │
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│                      16 Custom Tools                         │
├──────────────────┬──────────────────┬───────────────────────┤
│  Backup Sets     │  Policies        │  Operations           │
├──────────────────┼──────────────────┼───────────────────────┤
│ create_table_    │ create_backup_   │ add_backup            │
│ backup_set       │ policy           │ list_backups          │
│ create_schema_   │ apply_backup_    │ restore_table         │
│ backup_set       │ policy           │ restore_schema        │
│ create_database_ │ suspend_backup_  │ restore_database      │
│ backup_set       │ policy           │ delete_backup         │
│ create_backup_   │ resume_backup_   │ add_legal_hold        │
│ set_with_policy  │ policy           │ remove_legal_hold     │
│ drop_backup_set  │                  │ list_backup_sets      │
└──────────────────┴──────────────────┴───────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│              Snowflake Stored Procedures                     │
│              BACKUP_AGENT_DB.BACKUP_TOOLS.*                  │
└─────────────────────────────────────────────────────────────┘
Usage
Interacting with the Agent
Navigate to Agents in Snowsight and select BACKUP_ASSISTANT, or query programmatically:
SELECT SNOWFLAKE.CORTEX.INVOKE_AGENT(
    'BACKUP_AGENT_DB.BACKUP_TOOLS.BACKUP_ASSISTANT',
    'Create a backup set for my_database.my_schema.customers table'
);

Example Conversations
Creating a Table Backup Set:

"Create a backup set called CUSTOMERS_BACKUPS for the table SALES_DB.PUBLIC.CUSTOMERS"

Setting Up Automated Backups:

"Create a daily backup policy with 30-day retention and apply it to CUSTOMERS_BACKUPS"

Restoring from Backup:

"List the backups in CUSTOMERS_BACKUPS and restore the most recent one to CUSTOMERS_RESTORED"

Managing Legal Holds:

"Add a legal hold to the backup from January 15th in CUSTOMERS_BACKUPS"

Cost Estimation
Backup storage is billed at approximately $23/TB/month. Use this query to monitor costs:
Important Notes
Restore Limitations: Backups can only be restored to new object names—you cannot overwrite existing objects.

Delete Order: Only the oldest backup without a legal hold can be deleted from a backup set.

Retention Lock: Backup sets with retention locks cannot be dropped while containing unexpired backups.

Legal Holds: Backups under legal hold are exempt from automatic expiration.

Dependencies: When backing up tables with dependencies (foreign keys, sequences), ensure referenced objects exist when restoring.

Troubleshooting
"Backup set not found"
Ensure you're using the correct backup set name and have appropriate permissions.

"Cannot delete backup"
Only the oldest backup without legal hold can be deleted. Check is_under_legal_hold column.

"Sequence not found after restore"
Table references external objects. Restore the dependency first or use schema/database-level backups.

File Structure
Contributing
Fork the repository
Create a feature branch
Submit a pull request
License
MIT License

Support
For issues and questions:

Create a GitHub issue
Snowflake Community Forums
Snowflake Support (for account-specific issues)
Changelog
v1.0.0 (2026-03-05)
Initial release
16 backup management tools
Support for table, schema, and database backups
Three pre-configured backup policies
Legal hold support for compliance

