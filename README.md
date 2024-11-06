# Custom Data Store to Descope Migration Tool

> If you’re looking to migrate from third-party services like Auth0, Cognito, or Firebase, check out our dedicated [Third Party Service Migration Tool repository](https://github.com/descope/descope-migration).

This tool provides an efficient way to migrate your users from any custom data store to Descope, whether you're using plain-text passwords or hashed passwords with various algorithms. We support both Node.js and Python implementations, with detailed instructions for each language, to ensure a seamless migration experience.

## 🚀 Overview

Migrating users to Descope from a custom data store is simplified with our comprehensive migration script. This tool is designed to work with various password storage formats, allowing you to securely and accurately transfer user data to Descope.

### Supported Password Types

Our tool supports migration of both plain-text and hashed passwords. If your custom data store uses hashed passwords, we support algorithms such as `bcrypt`, `PBKDF2`, `Django`, and more. This flexibility allows you to retain your current password configurations and avoid requiring users to reset their passwords.

### Multi-Language Support

You can run the migration in either Node.js or Python, depending on your environment and preferences:

- **Node.js**: Offers a JavaScript-based approach with full compatibility for Descope’s API.
- **Python**: Provides a Pythonic migration experience that integrates seamlessly with Descope’s User Management API.

## 🛠️ Setup

### Node.js

1. Clone the repository and navigate to the Node.js folder.
2. Follow the [Node.js README](./node) for detailed setup instructions, including environment configuration and usage.

### Python

1. Clone the repository and navigate to the Python folder.
2. Refer to the [Python README](./python) for detailed setup instructions, including environment configuration and usage.

Both versions are designed to handle different hashing algorithms and will prompt you for any specific configurations needed for your data store.

## 🔑 Password Migration

> Learn more about Descope's password hashing algorithm support on our [docs page](https://docs.descope.com/migrate/custom#importing-passwords)

For password migration, our tool supports a wide range of hashing algorithms. If using plain-text passwords, the tool will automatically hash passwords in Descope-compatible formats. If using hashed passwords, ensure that you configure the tool with your hashing algorithm (e.g., `UserPasswordDjango`, `bcrypt`) to maintain compatibility with Descope’s User Management API.

Supported algorithms include:

1. **bcrypt**
2. **PBKDF2**
3. **Django**
4. **Firebase**
5. **PHPass**
6. **MD5** (for legacy compatibility)

With this flexibility, your users will be able to sign in with their existing passwords seamlessly after migration.

## 📜 License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for more details.
