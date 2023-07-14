# raspberry-pi-4
Raspberry Pi 4 Setup Notes

[Installing MongoDB](https://www.mongodb.com/developer/products/mongodb/mongodb-on-raspberry-pi/)

[Automate Continuous MongoDB to S3](https://www.mongodb.com/developer/products/atlas/automated-continuous-data-copying-from-mongodb-to-s3/)
```bash
#!/bin/bash

VERSION=1.20.6

# Download the latest version of Golang
wget https://dl.google.com/go/go$VERSION.linux-armv6l.tar.gz

# Extract the archive
tar -C /usr/local -xzf go$VERSION.linux-armv6l.tar.gz

# Set the environment variable for Golang
echo 'export PATH=$PATH:/usr/local/go/bin' >> ~/.zshrc
source ~/.zshrc

# Verify the installation
go version

# Clean up
rm go$VERSION.linux-armv6l.tar.gz
```
