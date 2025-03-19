```markdown
# Monerod YunoHost App Package

This repository contains a YunoHost-compliant app package for installing and running the Monero daemon (`monerod`) from source. It builds monerod from the official Monero repository and lets administrators configure key settings such as the blockchain data directory, network ports, and whether to enable pruned mode. The package integrates seamlessly with YunoHost’s app management system for service control, firewall configuration, and upgrade/uninstall handling.

## Features

- **Build from Source:** Downloads and compiles monerod from the official Monero repository.
- **Custom Configuration:** Prompt-based configuration for:
  - **Data Directory:** Path for storing blockchain data.
  - **P2P Port:** Port for peer-to-peer connections (default: 18080).
  - **RPC Port:** Port for local RPC calls (default: 18081).
  - **Pruned Mode:** Option to run a pruned node to save disk space.
- **YunoHost Integration:** Uses YunoHost helpers to manage system users, systemd services, firewall rules, and app settings.
- **Upgrade & Removal:** Provides scripts to upgrade monerod with minimal downtime and to uninstall the package cleanly.

## Package Structure

```
.
├── manifest.toml         # App metadata, configuration questions, and resource definitions
├── scripts
│   ├── install           # Installation script (builds and installs monerod)
│   ├── upgrade           # Upgrade script (updates the monerod binary)
│   └── remove            # Uninstallation script (removes the app and its configurations)
└── conf
    └── systemd.service   # Template for systemd service configuration
```

## Installation

### Prerequisites

- A running YunoHost server.
- Sufficient disk space for the blockchain data and build process.
- Internet access for downloading source code and build dependencies.

### Steps

1. **Using the YunoHost CLI:**

   ```bash
   yunohost app install --source https://github.com/yourusername/monerod-ynh.git
   ```

2. **Using the YunoHost Web Interface:**

   - Log in to your YunoHost admin panel.
   - Navigate to the **Applications** section.
   - Click **Install** and provide the repository URL for this package.

3. **Configuration Prompts:**

   During installation, you will be prompted for:
   - **Data Directory:** Default is `/home/yunohost.app/monerod`.
   - **P2P Port:** Default is `18080`.
   - **RPC Port:** Default is `18081`.
   - **Pruned Mode:** Enable (`true`) or disable (`false`) pruned mode (default is `false`).

## How It Works

- **Build Process:** The installation script downloads the Monero source code from the official GitHub repository at a specified release tag, then compiles it in release mode.
- **Service Management:** A systemd service is created (using a template in `conf/systemd.service`) to run monerod under a dedicated system user. The service binds the P2P port to all interfaces and the RPC port to localhost.
- **Firewall:** The package automatically opens the P2P port on the firewall to allow incoming connections.
- **Persistence:** App settings (such as the chosen ports and data directory) are stored via YunoHost, ensuring that configuration persists across upgrades.

## Upgrading

The provided upgrade script will:
- Stop the running monerod service.
- Build the latest version of monerod from the source.
- Replace the old binaries with the new ones.
- Restart the service while preserving all configuration and blockchain data.

To upgrade, run:
```bash
yunohost app upgrade monerod
```

## Uninstallation

The removal script stops the service, cleans up the installation directory, and reverts the firewall settings. Note that the blockchain data directory is preserved by default, so you can choose to remove it manually if needed.

To uninstall, run:
```bash
yunohost app remove monerod
```

## Contributing

Contributions are welcome! If you have suggestions or improvements:
1. Fork the repository.
2. Create a feature branch.
3. Commit your changes with clear commit messages.
4. Open a pull request describing your changes.

## License

This project is licensed under the MIT License, Monero is under the BSD-3-Clause license. See the [LICENSE](LICENSE) file for details.

## Acknowledgments

- [Monero Project](https://www.getmonero.org) for the Monero source code and build instructions.
- [YunoHost](https://yunohost.org) for providing a robust framework for packaging and managing self-hosted applications.
- The YunoHost community for their continuous support and contributions.

```

This README provides an overview of the package, detailed instructions for installation, and guidance for future upgrades or contributions. Enjoy running your Monero node with YunoHost!
