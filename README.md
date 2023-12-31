# ftp-cicd-deploy

```
name: ci/cd by guitar

on:
  push:
    branches:
      - 'main'

jobs:
  web-deploy:
    name: 🎉 Deploy
    runs-on: ubuntu-latest
    steps:
    - name: 🚚 Get latest code
      uses: actions/checkout@v3

    - name: Use Node.js 18
      uses: actions/setup-node@v2
      with:
        node-version: '18'
      
    - name: 🔨 Build Project
      run: |
        yarn
        yarn build
    
    - name: 📂 Sync files
      uses: SamKirkland/FTP-Deploy-Action@v4.3.4
      with:
        server: ${{ secrets.FTP_SERVER }}
        username: ${{ secrets.FTP_USERNAME }}
        password: tu[10UzBL4V*1o
        local-dir: dist/
        server-dir: domains/mateworker.com/public_html/
  connect-to-server:
    needs: web-deploy
    runs-on: ubuntu-latest
    steps:
      - name: Noti to discord
        uses: appleboy/ssh-action@v0.1.9
        with:
          host: ${{ secrets.HOST }}
          username: ${{ secrets.USERNAME_SSH }}
          password: ${{ secrets.PASSWORD_SSH }}
          port: ${{ secrets.PORT_SSH }}
          script: |
            /home/guitar/.nvm/versions/node/v18.15.0/bin/node '${{ secrets.NOTIFICATION }}'
```
