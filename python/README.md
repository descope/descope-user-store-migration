# Python User Migration (Custom Data Store)

This Python script allows you to migrate users from a custom data store to Descope, supporting both plain-text and hashed passwords (e.g., PBKDF2, bcrypt). It’s configurable to handle user roles, email verification status, and custom attributes.

## 📄 CSV Input Format

The migration script expects a CSV file containing the following fields:

| Field           | Description                                                                                            |
| --------------- | ------------------------------------------------------------------------------------------------------ |
| `identifier`    | Unique identifier for the user (e.g., email or username).                                              |
| `email`         | User's email address, used for login and communication.                                                |
| `verifiedEmail` | Boolean field indicating if the email is already verified.                                             |
| `displayName`   | Display name of the user, typically used in the UI.                                                    |
| `roleNames`     | Array of roles associated with the user (e.g., `admin`, `user`).                                       |
| `password`      | User's password hash. Supported formats include plain text or specific hashing algorithms like PBKDF2. |

### Example CSV Input

Here’s a sample CSV input to demonstrate the expected format:

```csv
identifier,email,verifiedEmail,displayName,roleNames,password
test1@example.com,test1@example.com,TRUE,Test One,[],pbkdf2_sha256$260000$qvuPAH7BzZx5oB278YWjNO$ydHrFpnbLr+udgJkquUusfG3YQBI6+ZvxsnVbdLserw=
test3@example.com,test3@example.com,TRUE,Test Three,"[""admin"", ""super_user""]",pbkdf2_sha256$260000$D60FxGGluDfVTANezkv6lO$8qUrthZYWiCn3YD3mc3ML9nVQd3FRi+jkpAQ0zmUGCY=
```

- **Password Field**: In this example, the passwords use the PBKDF2 hashing format as stored in Django. The format includes the hashing algorithm, iteration count, salt, and hash.
- **RoleNames Field**: Roles should be specified as an array within quotes (e.g., `["admin", "super_user"]`).

## ⚙️ Setup

### 1. Add Environment Variables

Set up the necessary Descope environment variables. Replace `<your descope project id>` and `<your descope management key>` with your actual values:

```sh
export DESCOPE_PROJECT_ID=<your descope project id>
export DESCOPE_MANAGEMENT_KEY=<your descope management key>
```

- **Project ID**: Can be found [here](https://app.descope.com/settings/project).
- **Management Key**: Can be found [here](https://app.descope.com/settings/company/managementkeys).

### 2. Run the Migration Script

1. **Create a virtual environment** to manage dependencies:

   ```sh
   python -m venv venv
   ```

2. **Activate the virtual environment**:

   - On MacOS and Linux:

     ```sh
     source venv/bin/activate
     ```

   - On Windows:

     ```sh
     .\venv\Scripts\activate
     ```

3. **Install dependencies**:

   ```sh
   pip install -r requirements.txt
   ```

4. **Run the migration script**:

   ```sh
   python -m src.main
   ```

This script will process the CSV file, transform the data to match Descope’s requirements, and call the Descope User Management API to create user records, assign roles, and migrate passwords.

## 🔄 Customization

The script can be customized to include additional fields by modifying the `user_obj` in `src/main.py`. Update the `user_obj` to include any other fields from your data source, such as custom attributes or metadata.

For example:

```python
user_obj_args = {
  "loginIds": [user_data["identifier"]],
  "email": user_data["email"],
  "name": user_data.get("displayName", ""),
  "roleNames": user_data.get("roleNames", []),
  "password": user_data["password"]
}
```
