# Define the Distinguished Name (DN) of the Paris OU
$ouDn = "OU=Paris,DC=contoso,DC=com"

# Define the name of the group
$groupName = "Paris Admins"

# Retrieve the security identifier (SID) of the group
$groupSid = (Get-ADGroup $groupName).SID

# Define the permissions to be assigned
$permissions = @(
    "Reset Password",
    "Change Password"
)

# Loop through each permission and add it to the Paris OU
foreach ($permission in $permissions) {
    $acl = Get-ACL -Path "AD:$ouDn"
    $ace = New-Object System.DirectoryServices.ActiveDirectoryAccessRule(
        $groupSid,
        $permission,
        "Allow"
    )
    $acl.AddAccessRule($ace)
    Set-ACL -Path "AD:$ouDn" -AclObject $acl
}
