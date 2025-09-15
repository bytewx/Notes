# Install Go

- golang.org/dl:
```
wget https://go.dev/dl/go1.23.1.linux-amd64.tar.gz
```
- Delete old version, if it exists:
```
sudo rm -rf /usr/local/go
```
- Unpack archive into `/usr/local`:
```
sudo tar -C /usr/local -xzf go1.23.1.linux-amd64.tar.gz
```
- Add Go to `PATH` (add lines to ~/.bashrc or ~/.zshrc):
```
export PATH=$PATH:/usr/local/go/bin
```
- Then accept the changes:
```
source ~/.bashrc
```
- Check version:
```
go version
```
