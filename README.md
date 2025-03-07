# S3 Backup

This repository contains scripts to automatically back up all of your S3-compatible buckets to your local machine on a scheduled basis. It works with:

- Amazon S3
- Cloudflare R2

## Features

- Automatically backs up all buckets from your S3-compatible service
- Configurable retention policy (default: keep the 3 most recent backups per bucket)
- Scheduled backups (default: every other week)
- Secure credential management using .env file (not committed to git)
- Works with any S3-compatible storage service

## Prerequisites

- macOS (Mac mini or other Mac)
- Homebrew (will be installed if not present)
- AWS CLI (will be installed via Homebrew if needed)
- Access credentials for your S3-compatible service

## Installation

1. Clone this repository to your Mac:

   ```bash
   git clone https://github.com/yourusername/s3-backup.git
   cd s3-backup
   ```

2. Run the setup script:

   ```bash
   chmod +x setup.sh
   ./setup.sh
   ```

3. Edit the `.env` file with your S3 credentials:

   ```bash
   nano .env
   ```

   Update the following variables:

   - `S3_ENDPOINT_URL`: Your S3 service endpoint URL
   - `S3_ACCESS_KEY`: Your access key
   - `S3_SECRET_KEY`: Your secret key
   - `S3_REGION`: Your S3 region (if applicable)
   - `LOCAL_BACKUP_DIR`: Where to store your backups (default: ~/backups/s3)
   - `MAX_BACKUPS`: Number of backups to retain per bucket (default: 3)

4. Make the backup script executable:
   ```bash
   chmod +x scripts/s3_backup.sh
   ```

## S3 Endpoint Configuration Examples

Here are endpoint URLs for the supported S3-compatible services:

### Amazon S3

```
S3_ENDPOINT_URL=https://s3.amazonaws.com
S3_REGION=us-east-1  # Replace with your region
```

### Cloudflare R2

```
S3_ENDPOINT_URL=https://accountid.r2.cloudflarestorage.com
S3_REGION=auto
```

## Usage

### Manual Backup

To run a backup manually:

```bash
./scripts/s3_backup.sh
```

### Scheduled Backups

The setup script automatically configures a cron job to run the backup every other Sunday at 2:00 AM.

To view the current cron job:

```bash
crontab -l
```

To modify the schedule:

```bash
crontab -e
```

## AWS CLI Installation

The setup script will automatically install the AWS CLI if it's not already present on your system. It uses Homebrew to handle the installation.

If you prefer to install it manually, you can run:

```bash
# Install Homebrew if not already installed
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"

# Install AWS CLI
brew install awscli

# Verify installation
aws --version
```

## Security

- **IMPORTANT**: Never commit your `.env` file to version control
- The `.gitignore` file is configured to exclude the `.env` file containing your credentials
- Only the `.env.template` should be committed to git

## Logs

Backup logs are stored in the same directory as the scripts, with filenames formatted as `backup_YYYYMMDD.log`.

## Troubleshooting

If you encounter issues:

1. Check the log files for error messages
2. Verify your S3 credentials in the `.env` file
3. Ensure the AWS CLI is installed correctly:
   ```bash
   aws --version
   ```
4. Test your connection to your S3 service:
   ```bash
   aws s3 ls --endpoint-url your-endpoint-url-here
   ```
5. If you're getting permission errors, verify your access key and secret key have the necessary permissions

## License

MIT
