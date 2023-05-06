
___
<br>

## Notes for the Udemy Course "**[Ultimate AWS Certified Developer Associate DVA-C02](https://www.udemy.com/share/101WgC3@DRbJZZoAoUIMoYUmM3u5Ke78QEJ9BP5f8qPUuyMpR6phqEGLaAqnz7zAl-GsFUH-/)**"
<br>

___

# 4️⃣ IAM & AWS CLI 
### IAM = Identity and Access Management (_Global Service_)
## 🟨 Consists of
- Root account (_Shouldn't be used or shared_)
- Users
- Groups
  - Can only contain users, not other groups 
  - Users can belong to multiple groups 
## 🟨 Policies (_Permissions_)
A JSON document that defines what a user/group is allowed to do
- Least privilege principle
  - Don't give more permission than a user needs 
- Inline policies
  - User-specific policies 
- Structure
  - Sid
    - Identifier for the statement (_optional_)
  - Effect
    - Whether to allow or deny access
      - Allow
      - Deny
  - Principle
    - To what this policy applies to
      - Account
      - User
      - Role
  - Action
    - List of actions allowed or denied
  - Resources
    - List of resources the actions are applied to
  - Condition
    - Conditions for when this policy is in effect (_optional_)
```JSON
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Action": [
                "iam:GetContextKeysForCustomPolicy",
                "iam:GetContextKeysForPrincipalPolicy",
                "iam:SimulateCustomPolicy",
                "iam:SimulatePrincipalPolicy"
            ],
            "Effect": "Allow",
            "Resource": "*"
        }
    ]
}
```
## 🟨 IAM Password Policy
- Minimum length 
- Require specific char types 
  - Include uppercase 
  - Include lowercase 
  - Numbers 
  - Non-alphanumeric  
- Allow all IAM users to change their own password 
- Password Expiration: Require users to change their password 
- Prevent password re-use
## 🟨 MFA (Multi Factor Authentication)
- Virtual MFA Device 
  - Google Authenticator 
  - Authy 
- U2F = Universal 2nd factor Security Key 
  - YubiKey by Yubico 3rd party 
- Hardware Key Fob MFA Device 
  - Provided by Gemalto 3rd party 
- Hardware Key Fob MFA Device for AWS GovCloud (US) 
  - Provided by SurePassID 3rd party
## 🟨 CLI = Command Line Interface 
- Access Keys are generated through AWS Console 
- Users manage their own access keys 
- Don't share access keys! 
  - Access Key ID ~ username 
  - Secret Access Key ~ Password 
## 🟨 CloudShell 
- A browser-based shell with AWS CLI access from the AWS Management Console
- Not available everywhere
## 🟨 IAM Roles 
- To be used by AWS Servies to perform actions 
- Common Roles 
  - EC2 Instance Roles 
  - Lambda Function Roles 
  - Roles for CloudFormation 
- Only Roles for AWS Services is required to know for the exam
## 🟨 IAM Security Tools 
- IAM Credentials Report (_account level_)
  - A report that lists all accounts users and the status of various credentials 
- IAM Access Advisor (_user level_)
  - Shows the service permissions granted to a user and when they were last accessed 
  - Use information to revise policies 
## 🟨 Best Practices
- Don't use the root account except for AWS account setup
- One physical user **==** One AWS user
- Assign users to groups and assign permissions to groups
- Create a strong password policy
- Use and enforce MFA
- Use Access Keys for programmatic access (_CLI/SDK_)
- Audit permissions of your account using
  - IAM Credentials Report
  - IAM Access Advisor
- Never share IAM users & access keys