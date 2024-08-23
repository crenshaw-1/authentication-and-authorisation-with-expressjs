# Authentication vs. Authorization: Analysis on User Deletion Functionality

## Introduction
The requirement "This delete user functionality can be done after authentication" raises important security concerns in the application. This is particularly problematic due to the understood concepts of authentication and authorization, and how they differ.

## Key Differences
### Authentication
Definition: The process of verifying a user's identity.
Purpose: Confirms that users are who they claim to be, e.g., Logging in with a username and password.

### Authorization
Definition: The process of determining what actions a user is allowed to perform.
Purpose: Controls access to resources and functionalities based on user permissions, e.g., Checking if a user has admin rights to delete other users.

## Is Deletion After Authentication a Good Idea ?
The requirement to allow user deletion after authentication alone is a bad idea, majorly due to the security risk. 

- Security: Authentication only verifies identity, not permissions. This could allow any authenticated user to delete any other user account.
- Lack of Access Control: Without authorization, there's no way to protect certain database documents or restrict deletion capabilities to specific roles (e.g., admins).
- Potential for Abuse: Malicious users could exploit this to remove other users from the system without means to track who performed deletions, complicating accountability.

A secure approach involves:
- Authenticate the user (verify identity)
- Authorize the user (check if they have permission to delete users)
- Perform the deletion only if both steps are successful
e.g.

```javaScript
router.post(
    "/delete/user",
    authentication,
    authorisation("admin"),
    (req, res) => authController.delete_user_by_username(req, res)
)
```

The route above applies both authentication and authorization middleware before allowing the delete operation, ensuring only authorized users (i.e., admins) can perform deletions.

## Conclusion
While authentication is crucial for identifying users, it's insufficient for managing sensitive operations like user deletion. Proper authorization must be implemented alongside authentication, to ensure that only users with appropriate permissions can perform such actions (It will also be advised to implement adequate error Handling). This two-step process significantly enhances the security and integrity of the application.