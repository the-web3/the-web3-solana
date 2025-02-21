# Solana Co-Learning

![Rust](https://img.shields.io/badge/Rust-1.75.0-orange?logo=rust)
![License](https://img.shields.io/badge/License-MIT-blue)

**Solana Co-Learning** 是一个开源项目，旨在帮助开发者和社区成员深入了解并实践 Solana 区块链的开发技术和最佳实践。通过本项目，您将获得构建和部署智能合约（特别是使用 Solana 的 Rust 和 Anchor 框架）以及与 Solana 网络交互的实战经验。

## 项目目标

- 提供系统化的 Solana 开发教程，涵盖智能合约的创建、部署、测试和优化。
- 推广 Solana 技术生态，帮助开发者快速上手并构建去中心化应用（DApp）。
- 集成 Solana 核心工具（如 Solana CLI、Anchor、Phantom 钱包）和性能优化实践。
- 鼓励社区成员贡献代码、分享知识和经验，共同进步。

## 功能

- **Solana 开发教程**：逐步指导，帮助您从零开始学习如何在 Solana 区块链上开发应用。
- **智能合约开发**：学习如何使用 Solana 的 Rust 编程语言和 Anchor 框架编写和部署智能合约。
- **钱包集成**：提供 Phantom 钱包与 Solana DApp 集成的实用指南，处理交易签名和钱包管理。
- **Token 创建与管理**：学习如何在 Solana 上创建自定义 Token，分发和高效管理这些 Token。
- **Solana 网络交互**：学习如何与 Solana 网络交互，查询账户余额、发送交易并处理错误。

## 安装

开始使用 Solana Co-Learning，按照以下步骤设置您的开发环境：

### 前提条件

1. **Rust**：确保您已经在机器上安装了 Rust。您可以通过 [rustup](https://www.rust-lang.org/tools/install) 安装。

   ```bash
   curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
   ```

2. **Solana CLI**：安装 Solana 命令行界面（CLI），以便与 Solana 区块链交互。

   - 跟随 [Solana CLI 安装指南](https://docs.solana.com/cli/install-solana-cli-tools) 进行安装。
   - 设置 Solana CLI 使用 **Devnet**：

     ```bash
     solana config set --url https://api.devnet.solana.com
     ```

3. **Anchor 框架**：安装 Anchor，这是一个简化 Solana 智能合约开发的框架。

   ```bash
   npm install -g @project-serum/anchor-cli
   ```

4. **Phantom 钱包**：安装 Phantom 钱包浏览器扩展，用于 Solana DApp 钱包集成。

### 设置项目

克隆本仓库到您的本地机器：

```bash
git clone https://github.com/yourusername/solana-co-learning.git
cd solana-co-learning
```

### 安装依赖

对于 Rust 依赖：

```bash
cd program
cargo build-bpf
```

对于 JavaScript 依赖（如果您的项目包含前端组件）：

```bash
npm install
```

### 运行项目

如果您有 Rust 和 JavaScript 组件：

1. **部署 Solana 智能合约**：

   ```bash
   anchor deploy
   ```

2. **启动本地开发服务器**（如果适用）：

   ```bash
   npm run dev
   ```

## 使用方法

设置好开发环境后，您可以开始在 Solana 上构建 DApp：

1. **开发智能合约**：使用 Anchor 框架在 Rust 中编写智能合约，学习如何定义程序、管理账户和处理错误。
2. **与 Solana 网络交互**：发送交易、查询账户并与 Solana 网络上的去中心化程序交互。学习如何管理状态、Token 和账户。
3. **钱包集成**：集成 Phantom 钱包，使用户可以通过签名交易和管理资产与您的 DApp 交互。
4. **Token 管理**：创建、分发和管理 Solana 上的 Token，了解 Solana Token 标准的工作原理。

## 学习资源

为了深入理解 Solana 开发和区块链编程，以下是一些有用的资源：

- [Solana 文档](https://docs.solana.com/) - 关于 Solana 的全面资源，从核心概念到高级特性。
- [Anchor 文档](https://www.anchor-lang.com/docs/) - 使用 Anchor 框架开发 Solana 智能合约的指南。
- [Solana](https://www.youtube.com/@SolanaFndn) - 官方 Solana 频道，提供教程、演讲和开发相关内容。

- [Solana CLI](https://docs.solana.com/cli) - 用于与 Solana 区块链交互的命令行工具。
- [Anchor 框架](https://www.anchor-lang.com/) - 用于在 Rust 中编写 Solana 智能合约的框架。
- [Phantom 钱包](https://www.phantom.app/) - 与 Solana DApp 互动的流行钱包。
- [Solana 开发者资源](https://github.com/CristinaSolana/solana-developer-resources)
- [Solana秘籍](https://solanacookbook.com/zh/)
- [Solana中文开发教程](https://www.solanazh.com/)
- [Solana Cookbook](https://solana.com/zh/developers/cookbook#contributing)
- [Solana Co-Learning#3](https://github.com/706creators/solana-co-learn?tab=readme-ov-file)

## Solana 浏览器

- [Solana Explorer](https://explorer.solana.com/) - 探索 Solana 区块链，查看交易，搜索账户和 Token。
- [Solana Explorer](https://solscan.io/) - 另一个 Solana 区块链浏览器，提供额外的功能和视图。

## 如何参与共学

欢迎加入 **Solana 共学项目**！请按照以下步骤参与并分享你的学习旅程：

### Step 01: Fork 本仓库

- 点击 GitHub 页面右上角的 `Fork` 按钮，将本仓库复制到你的 GitHub 账户下。

### Step 02: 创建个人笔记文件

1. 在你 Fork 的仓库中，复制 `template` 文件夹。
2. 将复制的文件夹重命名为你的 GitHub ID（例如：`YourGitHubID`）。
3. 打开重命名后的文件夹中的 `Readme.md` 文件，填写你的个人信息（例如姓名、GitHub ID、学习目标）。
4. 在该文件夹下记录你的学习过程：
   - `code`：存放代码示例。
   - `note`：记录文字笔记。
   - `img`：添加相关图片。

### Step 03: 提交 Pull Request (PR)

- 将你的更改提交到你的 Fork 仓库。
- 在 GitHub 上发起一个 Pull Request (PR)，将你的个人文件夹合并到主仓库。
- 我们的团队将审核并合并你的贡献！

通过分享你的笔记，你将帮助他人从你的经验中学习，同时加深自己对 Solana 开发的理解。

## 贡献

我们欢迎大家的贡献！无论您是修复 Bug、添加新功能，还是改进文档，您的贡献都能让 **Solana Co-Learning** 变得更好。请遵循 [CONTRIBUTING.md](CONTRIBUTING.md) 文件中的指南，了解如何贡献。

## 许可证

本项目遵循 [MIT 许可证](LICENSE)。

## 联系

如有任何问题或需要支持，欢迎随时打开 issue 或直接联系维护者。

- [GitHub Issues](https://github.com/the-job-org/solana-co-learning/issues)
- [Solana 开发者文档](https://docs.solana.com/)
- [Anchor 文档](https://www.anchor-lang.com/docs/)

---

感谢您为 **Solana Co-Learning** 的贡献！让我们一起构建去中心化应用的未来！
