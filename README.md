# Documentation of the Twona API v2.

## Index
- **General**
    - [Basic description](#basic-description)
    - [Operation](#operation)
- **Users**
    - [Invite a new user (POST)](#invite-a-new-user)
    - [Get all invitations (GET)](#get-all-invitations)
    - [Cancel an invitation (DELETE)](#cancel-an-invitation)
    - [Get all users (GET)](#get-all-users)
    - [Get one user (GET)](#get-one-user)
    - [Update one user (PUT)](#update-one-user)
    - [Deactivate one user (DELETE)](#deactivate-one-user)
    - [Activate one user (POST)](#activate-one-user)
    - [Add users bulk (POST)](#add-users-bulk)
- **Groups**
    - [Add group (POST)](#add-group)
    - [Get one group (GET)](#get-one-group)
    - [Get all groups (GET)](#get-all-groups)
    - [Update one group (PUT)](#update-one-group)
    - [Deactivate one group (DELETE)](#deactivate-one-group)
    - [Activate one group (POST)](#activate-one-group)
    - [Get users in group (GET)](#get-users-in-group)
    - [Add one user to group (POST)](#add-one-user-to-group)
    - [Deactivate one user in group (DELETE)](#deactivate-one-user-in-group)
- **Uploading files**
    - [Request upload a file (POST)](#requesting-upload-a-file)
    - [Uploading content (PATCH)](#uploading-content)
    - [Check upload (HEAD)](#check-upload)
- **Files**
    - [Get a file (GET)](#get-a-file)
    - [Download a file (GET)](#download-a-file)
    - [Add metadata to File (PUT)](#add-metadata-to-file)
    - [Get all organization's files (GET)](#get-all-organizations-files)
- **Auto-key login (only with special permission)**
    - [Request a magic link (POST)](#request-a-magic-link)
- **Filtering**
    - [Search queries](#search-queries)

## Basic description

The API will provide a full management of users and groups of users within your organization.

## Operation
The API manages the new users **invitations**, the team **users**, the team **groups**, **users within groups** and **users files** and working with them. We will separate each action request in this documentation.
