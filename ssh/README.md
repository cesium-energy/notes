# SSH, SCP, SFTP
## SSH
```shell
ssh -i identity_file user@host -p port
```

## SCP
```shell
# General syntax
scp <SOURCE> <TARGET>

# Upload file
scp /path/to/local/file user@host:/target/path

# Download file
scp user@host:/target/path /path/to/local/file

# Copy whole directory
scp -r /path/to/local/directory user@host:/target/path
```

## SFTP
```shell
sftp -i identity_file user@host -P port

# Download file
sftp> lcd local_target_dir
sftp> cd remote_source_dir
sftp> get filename

# Upload file
sftp> lcd local_source_dir
sftp> cd remote_target_dir
sftp> put filename

# Terminate session
sftp> bye
```