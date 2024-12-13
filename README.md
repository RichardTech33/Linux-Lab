# RHCSA-style Linux Project: User and Directory Configuration
### Description: Automates the creation of a user, shared directory setup, and configuration of password expiration policies.

# Exit on error
set -e

# Variables
USERNAME="john"
GROUPNAME="developers"
HOMEDIR="/home/$USERNAME"
SHAREDDIR="/shared/projects"
PASSWORD_EXPIRY_DAYS=30
PASSWORD_WARN_DAYS=7

## 1. Create the group
# Adds a group called 'developers' if it doesn't exist.
echo "Creating group: $GROUPNAME"
groupadd $GROUPNAME

## 2. Create the user
# Adds a user called 'john' with a home directory and assigns them to the 'developers' group.
echo "Creating user: $USERNAME"
useradd -m -d "$HOMEDIR" -g "$GROUPNAME" "$USERNAME"

## 3. Set a password for the user
# Requires manual input to set the user's password.
echo "Setting password for user: $USERNAME"
passwd "$USERNAME"

## 4. Create the shared directory
# Creates a shared directory at '/shared/projects' if it doesn't already exist.
echo "Creating shared directory: $SHAREDDIR"
mkdir -p "$SHAREDDIR"

## 5. Set ownership and permissions on the shared directory
# Assigns the 'developers' group to the directory and sets restrictive permissions.
echo "Setting group ownership and permissions for $SHAREDDIR"
chgrp "$GROUPNAME" "$SHAREDDIR"
chmod 770 "$SHAREDDIR"

## 6. Configure password expiration policies
# Sets password expiration to 30 days and warns 7 days before expiry.
echo "Configuring password expiration policies for user: $USERNAME"
chage -M "$PASSWORD_EXPIRY_DAYS" -W "$PASSWORD_WARN_DAYS" "$USERNAME"

## 7. Verification Outputs
# Outputs information for verification steps.

### Verify user and group
id "$USERNAME"

### Verify directory permissions
ls -ld "$SHAREDDIR"

### Verify password policies
chage -l "$USERNAME"

## Final message
echo "Setup completed successfully!"

