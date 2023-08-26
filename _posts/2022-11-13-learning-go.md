# Learning Go

## Get dev environment ready
### Install Go on Mac (with homebrew)

Install go.
```bash
brew install golang
```

Setup workspace.
```bash
mkdir -p $HOME/go/{bin,src,pkg}
```

Setup environment
```bash
export GOPATH=$HOME/go
export GOROOT="$(brew --prefix golang)/libexec"
export PATH="$PATH:${GOPATH}/bin:${GOROOT}/bin"

source $HOME/.zshrc
```

### Go version manager (GVM)
```bash
bash < <(curl -s -S -L https://raw.githubusercontent.com/moovweb/gvm/master/binscripts/gvm-installer)
```
```bash
gvm listall
gvm install go1.18.0
gvm use go1.18.0 [--default]
```
