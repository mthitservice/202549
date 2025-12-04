# Installation Ubuntu

``` bash
sudo apt update
sudo apt install apt-transport-https ca-certificates curl software-properties-common lsb-release -y

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

```
## Docker Repository hinzufügen

```bash
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] \
  https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

```

## Paketliste aktuallisieren und Docker installieren
```
sudo apt update
sudo apt install docker-ce docker-ce-cli containerd.io -y
```

## Docker Dienst aktivieren und starten
```bash
sudo systemctl enable docker
sudo systemctl start docker
```

## (Optional) Docker ohne sudo
```bash
sudo usermod -aG docker $USER
```
