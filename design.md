# Project: Husky PWA

## Overview
Build a Progressive Web Application for mushers and dog trip companies to manage husky dogs, log trips, track health journals, and map relationships between dogs. Persistent data is stored with Airtable as the backend database.

---

## Authentication

Use **email + password** authentication (keep it simple for now).
- Email verification optional
- JWT tokens stored securely in HTTP-only cookies
- Session timeout: 30 days
- Password reset via email link


All users have the same role.
  
---

## Core Features

### 1. User Management
- Create account with email + password
- Profile settings (name, visibilty)
- All users have equal permissions

### 2. Husky Management
Each user can add their own huskies. Each dog record includes:

| Field | Type | Required |
|-------|------|----------|
| Name | Text | ✓ |
| Gender | Select (Male/Female/Unknown) | ✓ |
| Notes | Text | ✗ |
| Weight | Number | ✗ |
| Photo | Attachment | ✗ |
| Creator | Link to Users table | ✓ |
| Visibility | Select (Private/Shared/Public) | ✓ |

### 3. Visibility & Sharing Logic

| Visibility Level | Who Can See | Who Can Edit |
|------------------|-------------|--------------|
| **Private** | Creator only | Creator only |
| **Friends** | Creator + Friends of Creator | Creator only |
| **Public** | Anyone (including guests/not logged in) | Creator only |

**Invitation System:**
- Creator shares dog ID via link or email invite
- Invited user accepts and gains edit access to that dog's records
- Permission hierarchy stored in a `Dog_Permissions` Airtable table

### 4. Dog Journal
Per-dog status info:

| Field | Type | Notes |
|-------|------|-------|
| Dog(s) | Link to Huskies | Multi-select allowed — can link multiple dogs |
| Date | Date |  |
| **Status** | Select | Options: Underweight / Overweight / Sick / Injured / Restricted / Other |
| Completed | Date | When status ended/resolved |
| Notes | Long Text | Medical observations, treatment details |
| Attachments | Attachment | Photos, lab results, vet documents |
| Author | Link to Users | Who entered the record |
| Visibility | Inherited from dog | Same sharing rules as parent dog |


### 5. Trip Logging
Record dog sledding trips (simplified):

| Field | Type | Notes |
|-------|------|-------|
| Date & Time | DateTime | ✓ (required) |
| Visibility | Select | ✓ (Public/Friends/Private) |
| Distance (km) | Number | ✗ (optional) |
| Duration (hrs) | Number | ✗ (optional) |
| Dogs Involved | Link to Huskies | At least 1 required |
| Mushers on Trip | Text | Optional — can be username OR free-form name string |
| Photos | Attachment | Optional trip documentation |
| Created By | Link to Users | Who created the trip |

### 6. Relationships (Pedigree Mapping)
Track genetic connections between dogs:

| Field | Type |
|-------|------|
| This Dog | Link to Huskies |
| Related Dog | Link to Huskies |
| Relationship Type | Select (Parent/Offspring/Sibling/Littermate) |

### 7. Public Directory (Guest Access)
- Guests (not logged in) can browse public dogs only
- Filter/search public dogs
- View dog profiles marked as Public
- Cannot edit any dogs without being logged in

---
