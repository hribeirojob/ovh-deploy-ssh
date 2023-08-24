# OVH Deploy Hosting Action

A GitHub Action to deploy the contents of a repository to an OVH Hosting Plan (with SSH support)

>This GitHub Action works best with the OVH Pro Hosting plan. It might work with other providers if their hosting product meet the reguirements listed below.

## Requirements

* OVH Account with an active [Pro Hosting](https://www.ovh.de/hosting/hosting-pro.xml) service (which has SSH-support)

  * A new user with SSH access to your hosting account (Web -> Hosting plans -> `<yourPlan>` -> FTP - SSH -> Create a FTP user) or via treminal in your server

  ```adduser my-user```

  * Grant the user SSH access after you created the account or via terminal in your server

  ```usermod -aG sudo my-user```

  * Ensure you configured the users home directory as the websites root directory or via treminal in your server

  ```usermod -d /opt my-user```

* A repository with your websites code

* Needed environment variables are configured and the SSH users name + password are available as GitHub secrets (`<yourRepository>` -> Settings -> Secrets -> New secret)

## Example usage

Inside your code base/repository create the directories `.github/workflows`. In the `workflows` folder create a file `main.yml` with the following content:

```yaml
on: [push]

jobs:
  deploy:
    runs-on: ubuntu-latest
    name: Deploy code
    steps:
    - name: Deploy to OVH hosting
      uses: hribeirojob/ovh-deploy-ssh@v0.1
      env:
        OVH_HOSTING_USER: ${{ secrets.OVH_HOSTING_USER }}
        OVH_HOSTING_PASSWORD: ${{ secrets.OVH_HOSTING_PASSWORD }}
        OVH_HOSTING_DOMAIN: ssh.clusterXXX.hosting.ovh.net
        REPOSITORY_NAME: my-repo
        REPOSITORY_URL: https://github.com/me/my-repo.git
```

Now simply adjust the last three listed environment variables. Visit your OVH hosting plans control panel to get the correct SSH domain.
Don't forget to create two secrets at your repository with the names `OVH_HOSTING_USER` + `OVH_HOSTING_PASSWORD` and the appropriate credentials as the value. Use a strong password!


