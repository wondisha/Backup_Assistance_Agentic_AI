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
