# Contributing to Solana Co-Learning

We are excited to have you contribute to the **Solana Co-Learning** project! Your contributions help make this project better for everyone. Whether you are fixing bugs, improving documentation, or adding new features, your contributions are valuable. Please follow the guidelines below to get started.

## Table of Contents

- [Contributing to Solana Co-Learning](#contributing-to-solana-co-learning)
  - [Table of Contents](#table-of-contents)
  - [Code of Conduct](#code-of-conduct)
  - [How to Contribute](#how-to-contribute)
    - [Bug Reports](#bug-reports)
    - [Feature Requests](#feature-requests)
    - [Pull Requests](#pull-requests)
  - [Development Setup](#development-setup)
  - [Commit Message Guidelines](#commit-message-guidelines)
  - [Code Style](#code-style)
  - [License](#license)

## Code of Conduct

Please note that this project follows the [Contributor Covenant Code of Conduct](https://www.contributor-covenant.org/). By participating in this project, you agree to abide by its terms.

We encourage a welcoming and respectful environment for all contributors. Let's make this a positive space!

## How to Contribute

### Bug Reports

If you find a bug, please follow these steps:

1. Check the [GitHub Issues](https://github.com/yourusername/solana-co-learning/issues) to see if the issue has already been reported.
2. If not, create a new issue by providing the following information:
   - A clear description of the issue.
   - Steps to reproduce the issue.
   - Expected vs actual behavior.
   - Any error messages or logs.

### Feature Requests

We welcome feature requests! If you have an idea for a feature you'd like to see, please:

1. Check the existing issues to see if someone else has already requested it.
2. If not, open a new feature request and describe the functionality you're proposing.
3. Be specific about the expected outcome and how it will benefit the project.

### Pull Requests

We love receiving pull requests! To make sure everything goes smoothly, please follow these steps:

1. **Fork** the repository and clone it to your local machine.

   ```bash
   git clone https://github.com/yourusername/solana-co-learning.git
   ```

2. **Create a new branch** for your work.

   ```bash
   git checkout -b feature/your-feature-name
   ```

3. **Make your changes** and ensure the code works as expected.

4. **Commit your changes** with a clear, concise commit message following our [Commit Message Guidelines](#commit-message-guidelines).

5. **Push your changes** to your forked repository.

   ```bash
   git push origin feature/your-feature-name
   ```

6. **Open a pull request** to the main repository from your forked branch. Ensure your pull request includes the following:
   - A description of what was changed.
   - Reference any relevant issue numbers.

We will review your pull request as soon as possible. Thank you for your contribution!

## Development Setup

Before contributing, you need to set up the development environment. Here's how:

1. Clone the repository to your local machine.

   ```bash
   git clone https://github.com/yourusername/solana-co-learning.git
   cd solana-co-learning
   ```

2. **Install dependencies**:

   ```bash
   npm install
   ```

3. **Set up Solana CLI**:

   - Install Solana CLI if you haven’t already: [Solana Installation Guide](https://docs.solana.com/cli/install-solana-cli-tools).
   - Set the network to **Devnet**:

     ```bash
     solana config set --url https://api.devnet.solana.com
     ```

4. **Start the development server** (if applicable):

   ```bash
   npm run dev
   ```

5. You can now begin making changes and testing locally.

## Commit Message Guidelines

We follow the [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/) standard for commit messages. Please adhere to the following format:

```
<type>(<scope>): <message>

<type>: feat, fix, docs, style, refactor, test, chore
<scope>: optional (e.g., "ui", "api", "docs")
<message>: short description of what was done
```

Example:

```
feat(wallet): add Phantom wallet support
fix(contract): correct token transfer calculation
docs(readme): update setup instructions
```

This helps maintain a clean, understandable git history.

## Code Style

We follow the following code style guidelines to ensure consistency throughout the project:

- **JavaScript/TypeScript**: Follow the [Airbnb JavaScript Style Guide](https://github.com/airbnb/javascript).
- **Rust**: Follow the official [Rust Style Guide](https://doc.rust-lang.org/1.0.0/style/).
- **General**: Use descriptive names for variables, functions, and classes. Ensure readability and clarity in all code.

## License

By contributing to the project, you agree to license your contributions under the project's existing license.

This project is licensed under the [MIT License](LICENSE).

---

Thank you for helping make **Solana Co-Learning** better! We appreciate your time and effort.
