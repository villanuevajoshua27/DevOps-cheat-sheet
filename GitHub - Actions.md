# GitHub Actions
This is simple CI/CD using appleboy

## Step 1: Add secrets to the repository
1. Go to repository settings and go to `Actions` under `Secrets and Variables`
2. Click `New repository secrets`

   - `HOST` : ip address of host
   - `USER` : user of the host
   - `SSH_PRIVATE_KEY` : ssh key of the host
   - `REGION` : region of the host (optional)
  
## Step 2: Add workflows
1. In your repository, create a new file `.github/workflows/file.yml`

   ```bash
    name: CI/CD Pipeline
    on:
      push:
        branches:
          - main
    jobs:
      deployment:
        runs-on: ubuntu-latest

        steps:
        - name: Checkout Code
          uses: actions/checkout@v3
    
        - name: Connect to the host
          uses: appleboy/ssh-action@v1.1.0
          with:
            host: ${{ secrets.HOST }}
            username: ${{ secrets.USER }}
            key: ${{ secrets.SSH_PRIVATE_KEY }}
            port: 22
            script: |
   
              # Exit actions if there is an error
              set -e

              # Add your script here to deploy your application

   ```

2. Check logs in workflows in `Actions` 

## Additional information

- If you want to add jobs in your workflow that depends in other jobs you can add `needs` command

  ```bash
  pulling_images:
    runs-on: ubuntu-latest
    needs: deployment
  ```

- If you need to run same commands in multiple host, you can add multiple host separeted by comma
  ```bash
  - name: Connect to the host
   uses: appleboy/ssh-action@v1.1.0
   with:
     host: ${{ secrets.HOST_A }}, ${{ secrets.HOST_B }}, ${{ secrets.HOST_C }}
     username: ${{ secrets.USER }}
     key: ${{ secrets.SSH_PRIVATE_KEY }}
     port: 22
  ```
---

## Github self-hosted runners command
- `sudo ./svc.sh install` = install
- `sudo ./svc.sh start` = start in background
- `sudo ./svc.sh stop` = 

## Reference
[appleboy/ssh-action](https://github.com/appleboy/ssh-action)
