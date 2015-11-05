# Installing MySQL on AWS EC2 Ubuntu instance

## Update

```
sudo apt-get update -y
sudo apt-get -y
```

## Installation

```
sudo apt-get install mysql-server mysql-client -y
```

## Configuration

Set up root password

```
sudo mysqladmin -u root -h localhost password 'your_secret_password'
```

