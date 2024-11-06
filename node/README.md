# Node.js User Migration (Custom Data Store)

This Node.js script enables you to migrate users from a custom data store to Descope, supporting both plain-text and hashed passwords. It is customizable to handle user roles, email verification status, and other user attributes.

## 📄 CSV Input Format

The migration script expects a CSV file with the following fields:

| Field           | Description                                                                                            |
| --------------- | ------------------------------------------------------------------------------------------------------ |
| `identifier`    | Unique identifier for the user (e.g., email or username).                                              |
| `email`         | User's email address, used for login and communication.                                                |
| `verifiedEmail` | Boolean field indicating if the email is verified.                                                     |
| `displayName`   | Display name of the user, typically used in the UI.                                                    |
| `roleNames`     | Array of roles associated with the user (e.g., `admin`, `user`).                                       |
| `password`      | User's password hash. Supported formats include plain text or specific hashing algorithms like PBKDF2. |

### Example CSV Input

```csv
identifier,email,verifiedEmail,displayName,roleNames,password
test1@example.com,test1@example.com,TRUE,Test One,[],pbkdf2_sha256$260000$qvuPAH7BzZx5oB278YWjNO$ydHrFpnbLr+udgJkquUusfG3YQBI6+ZvxsnVbdLserw=
test2@example.com,test2@example.com,FALSE,Test Two,[],
test3@example.com,test3@example.com,TRUE,Test Three,"[""admin"", ""super_user""]",pbkdf2_sha256$260000$D60FxGGluDfVTANezkv6lO$8qUrthZYWiCn3YD3mc3ML9nVQd3FRi+jkpAQ0zmUGCY=
```

- **Password Field**: Supports hashed passwords in formats like PBKDF2. The `password` field should be either blank or contain the hashed password string.
- **RoleNames Field**: Specify roles as a JSON array, e.g., `["admin", "super_user"]`.

## ⚙️ Setup

### 1. Add Environment Variables

Set up the necessary Descope environment variables. Replace `<your descope project id>` and `<your descope management key>` with your actual values:

```sh
export DESCOPE_PROJECT_ID=<your descope project id>
export DESCOPE_MANAGEMENT_KEY=<your descope management key>
```

- **Project ID**: Can be found [here](https://app.descope.com/settings/project).
- **Management Key**: Can be found [here](https://app.descope.com/settings/company/managementkeys).

### 2. Install Dependencies and Run the Script

Install dependencies and start the migration script:

```sh
npm install
npm start
```

The script will process your CSV data, format it to Descope’s requirements, and use the Descope User Management API to create user records, assign roles, and migrate passwords.

## 🔑 Password Migration

To migrate passwords, the script can handle both plain-text and hashed passwords. By default, it expects a plain-text password, but you can modify it to use hashed passwords, like this:

```javascript
if (user.password) {
  // Use this block if you’re importing hashed passwords
  body.hashedPassword = {
    django: {
      hash: user.password,
    },
  };
  // Uncomment for plain-text passwords
  // body.password = user.password;
}
```

Replace `django` with the correct algorithm if you’re using a different format (e.g., `bcrypt`, `pbkdf2`). For more information on supported hashing algorithms, refer to [Descope’s documentation](https://docs.descope.com/migrate/custom#importing-passwords).

## 📄 Using the `APIUser` Type and Adding Custom Attributes

The `APIUser` type defines the structure of a user object. You can use it to include additional attributes like `phone` and `customAttributes`.

Here’s how you can construct the `body` object in the script using `APIUser`:

```javascript
let body: APIUser = {
  loginId: user.email,
  email: user.email,
  displayName: user.displayName,
  verifiedEmail: user.verifiedEmail,
};

// Add optional attributes
if (user.phone) {
  body.phone = user.phone; // Add phone if available
}

if (user.customAttributes) {
  body.customAttributes = {
    preferredLanguage: user.customAttributes.language,
    timezone: user.customAttributes.timezone,
  };
}

if (user.password) {
  // Use hashed passwords if necessary
  body.hashedPassword = {
    django: {
      hash: user.password,
    },
  };
  // Uncomment for plain-text passwords
  // body.password = user.password;
}
```

### Explanation of Additional Attributes

- **Phone**: Add `phone` if present in the CSV data. It must be a string.
- **Custom Attributes**: Use `customAttributes` to include any additional key-value pairs, such as `preferredLanguage` or `timezone`. Adjust the structure based on the fields in your CSV.

### Example Updated CSV with Additional Attributes

To migrate with these custom attributes, update the CSV to include columns for `phone` and `customAttributes`, if needed:

```csv
identifier,email,verifiedEmail,displayName,roleNames,password,phone,customAttributes
test1@example.com,test1@example.com,TRUE,Test One,[],pbkdf2_sha256$260000$qvuPAH7BzZx5oB278YWjNO$ydHrFpnbLr+udgJkquUusfG3YQBI6+ZvxsnVbdLserw=,1234567890,"{""language"": ""en"", ""timezone"": ""UTC""}"
test2@example.com,test2@example.com,FALSE,Test Two,[],,0987654321,"{""language"": ""es"", ""timezone"": ""PST""}"
```

With these updates, you can easily customize and extend the user migration process to handle various attributes beyond the basic setup, ensuring that all relevant data is transferred accurately to Descope.
