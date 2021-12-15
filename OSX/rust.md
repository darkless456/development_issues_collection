# rust configuration issues

## `ld: library not found for -lmysqlclient`

install diesel-cli

```bash
cargo install diesel_cli --no-default-features --features mysql
```

```bash
brew install mysql-client

# if zsh
echo 'export PATH="/usr/local/opt/mysql-client/bin:$PATH"' >> ~/.zshrc
source ~/.zshrc

# else
echo 'export PATH="/usr/local/opt/mysql-client/bin:$PATH"' >> ~/.bash_profile
source ~/.bash_profile
```
