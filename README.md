# Solana Co-Learning

![Rust](https://img.shields.io/badge/Rust-1.75.0-orange?logo=rust)
![License](https://img.shields.io/badge/License-MIT-blue)

**Solana Co-Learning** is an open-source project designed to help developers and community members deeply understand and practice Solana blockchain development techniques and best practices. Through this project, you will gain hands-on experience in building and deploying smart contracts (particularly using Solana's Rust and Anchor framework) and interacting with the Solana network.

**Note**: To view this README in **Chinese**, please refer to the [Chinese version here](./README.zh.md).

## Project Objectives

- Provide a structured Solana development tutorial covering smart contract creation, deployment, testing, and optimization.
- Promote the Solana tech ecosystem and help developers quickly onboard and build decentralized applications (DApps).
- Integrate core Solana tools (such as Solana CLI, Anchor, Phantom wallet) and performance optimization practices.
- Encourage community members to contribute code, share knowledge, and collaborate on improvements.

## Features

- **Solana Development Tutorials**: Step-by-step guides to learn how to develop applications on the Solana blockchain from scratch.
- **Smart Contract Development**: Learn how to write and deploy smart contracts using Solana’s Rust programming language and the Anchor framework.
- **Wallet Integration**: A practical guide on integrating Phantom wallet with Solana DApps, handling transaction signing, and wallet management.
- **Token Creation & Management**: Learn how to create custom tokens on Solana, distribute them, and manage them efficiently.
- **Solana Network Interaction**: Learn how to interact with the Solana network by querying account balances, sending transactions, and handling errors.

## Installation

To get started with Solana Co-Learning, follow these steps to set up your development environment:

### Prerequisites

1. **Rust**: Ensure that you have Rust installed on your machine. You can install it using [rustup](https://www.rust-lang.org/tools/install).

   ```bash
   curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
   ```

2. **Solana CLI**: Install the Solana Command Line Interface (CLI) to interact with the Solana blockchain.

   - Follow the [Solana CLI Installation Guide](https://docs.solana.com/cli/install-solana-cli-tools).
   - Set the Solana CLI to use **Devnet**:

     ```bash
     solana config set --url https://api.devnet.solana.com
     ```

3. **Anchor Framework**: Install Anchor, a framework to simplify Solana smart contract development.

   ```bash
   npm install -g @project-serum/anchor-cli
   ```

4. **Phantom Wallet**: Install the Phantom wallet extension in your browser for Solana DApp wallet integration.

### Setting Up the Project

Clone this repository to your local machine:

```bash
git clone https://github.com/yourusername/solana-co-learning.git
cd solana-co-learning
```

### Install Dependencies

For Rust dependencies:

```bash
cd program
cargo build-bpf
```

For JavaScript dependencies (if your project includes frontend components):

```bash
npm install
```

### Run the Project

If you have both Rust and JavaScript components:

1. **Deploy the Solana smart contract**:

   ```bash
   anchor deploy
   ```

2. **Start the local development server** (if applicable):

   ```bash
   npm run dev
   ```

## Usage

Once your development environment is set up, you can start building DApps on Solana:

1. **Developing Smart Contracts**: Use the Anchor framework to write smart contracts in Rust. Learn how to define programs, manage accounts, and handle errors.

2. **Interacting with the Solana Network**: Send transactions, query accounts, and interact with decentralized programs on Solana's network. Learn how to manage state, tokens, and accounts.

3. **Wallet Integration**: Integrate Phantom wallet for users to interact with your DApp by signing transactions and managing their assets.

4. **Token Management**: Create, distribute, and manage Solana-based tokens, understanding the mechanics of Solana’s token standard.

## Learning Resources

To deepen your understanding of Solana development and blockchain programming, here are some helpful resources:

- [Solana Documentation](https://docs.solana.com/) - Comprehensive resource for all things Solana, from core concepts to advanced features.
- [Anchor Documentation](https://www.anchor-lang.com/docs/) - A guide to using the Anchor framework for Solana smart contract development.
- [Solana](https://www.youtube.com/@SolanaFndn) - The official Solana channel with tutorials, talks, and development-related content.

- [Solana CLI](https://docs.solana.com/cli) - Command-line tools for interacting with the Solana blockchain.
- [Anchor Framework](https://www.anchor-lang.com/) - A framework for writing Solana smart contracts in Rust.
- [Phantom Wallet](https://www.phantom.app/) - A popular wallet for interacting with Solana-based DApps.
- [Solana Developer Resources](https://github.com/CristinaSolana/solana-developer-resources)
- [Solana秘籍](https://solanacookbook.com/zh/)
- [Solana中文开发教程](https://www.solanazh.com/)
- [Solana Cookbook](https://solana.com/zh/developers/cookbook#contributing)
- [Solana Co-Learning#3](https://github.com/706creators/solana-co-learn?tab=readme-ov-file)

## Solana Explorer

- [Solana Explorer](https://explorer.solana.com/) - Explore the Solana blockchain, view transactions, and search for accounts and tokens.
- [Solana Explorer](https://solscan.io/) - Another explorer for Solana, offering additional features and insights.

## How to Co-Learn

Welcome to the **Solana Co-Learning** project! Follow these steps to participate and contribute your learning journey:

### Step 01: Fork the Repository

- Click the `Fork` button at the top-right of the GitHub page to create a copy of this repository under your GitHub account.

### Step 02: Create Your Personal Notes

1. In your forked repository, duplicate the `template` folder.
2. Rename the duplicated folder to your GitHub ID (e.g., `YourGitHubID`).
3. Open the `Readme.md` file inside your renamed folder and fill in your personal information (e.g., name, GitHub ID, learning goals).
4. Use this folder to document your learning journey:
   - `code`: Store your code examples.
   - `note`: Record your text-based notes.
   - `img`: Add relevant images.

### Step 03: Submit a Pull Request (PR)

- Commit your changes to your forked repository.
- Create a Pull Request (PR) on GitHub to merge your personal folder into the main repository.
- Our team will review and merge your contribution!

By sharing your notes, you’ll help others learn from your experience while deepening your own understanding of Solana development.

## Contributing

We welcome contributions from everyone! Whether you're fixing bugs, adding new features, or improving documentation, your contributions help make **Solana Co-Learning** better. Please follow the guidelines outlined in the [CONTRIBUTING.md](CONTRIBUTING.md) file for detailed instructions on how to contribute.

## License

This project is licensed under the [MIT License](LICENSE).

## Contact

For questions or support, please feel free to open an issue or reach out directly to the maintainers.

- [GitHub Issues](https://github.com/the-job-org/solana-co-learning/issues)
- [Solana Developer Documentation](https://docs.solana.com/)
- [Anchor Documentation](https://www.anchor-lang.com/docs/)

---

Thanks for contributing to **Solana Co-Learning**! Let's build the future of decentralized applications together!
