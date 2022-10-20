**Ubuntu**

**To install  Terraform on Ubuntu, please use the following code:**

```bash
curl -fsSL https://apt.releases.hashicorp.com/gpg | sudo apt-key add -
sudo apt-add-repository "deb [arch=$(dpkg --print-architecture)] https://apt.releases.hashicorp.com $(lsb_release -cs) main"
sudo apt install terraform
```

**MacOS**

**To install  Terraform on MacOS, please use the following code:**

```bash
brew tap hashicorp/tap
brew install hashicorp/tap/terraform
```

**Windows**

To install  Terraform on Windows, there are 2 possibilities:

*Chocolatey*:

```
`choco install terraform`
```

*Manual Instalation*
Download the [file](https://www.terraform.io/downloads.html) and select the correct version to be install,
