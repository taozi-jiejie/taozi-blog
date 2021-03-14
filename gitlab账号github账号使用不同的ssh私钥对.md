> 原本为gitbal生成了一对ssh公私钥对，现在想为github账号生成另一对ssh密钥对

### github生成ssh密钥对

1. 设置github使用的邮箱和用户名

   ```
   Administrator@SC-202012071432 MINGW64 ~
   $ git config --global user.name 用户名
   
   Administrator@SC-202012071432 MINGW64 ~
   $ git config --global user.email 邮箱地址
   
   ```

2. 生成ssh私钥对

   注意在c/user/用户/.ssh下已经有默认的ssh名称了，这个时候要换个公私钥名称

   ```
   Administrator@SC-202012071432 MINGW64 ~
   $ ssh-keygen -t rsa -C liyunyan1105@foxmail.com
   Generating public/private rsa key pair.
   Enter file in which to save the key (/c/Users/Administrator/.ssh/id_rsa): C:\Users\Administrator\.ssh\id_rsa_github
   ```

3. 由于ssh默认只识别默认名称的密钥文件，使用自定义名称时需要将密钥文件加入ssh-agent中

   此处直接`ssh-add .ssh/id_rsa_github`会出现权限问题

   ```
   Administrator@SC-202012071432 MINGW64 ~
   $ ssh-add .ssh/id_rsa_github
   Could not open a connection to your authentication agent.
   
   Administrator@SC-202012071432 MINGW64 ~
   $ ssh-agent bash
   
   Administrator@SC-202012071432 MINGW64 ~
   $ ssh-add .ssh/id_rsa_github
   Identity added: .ssh/id_rsa_github (liyunyan1105@foxmail.com)
   ```

4. 创建配置文件

   进入.ssh目录（密钥所在目录）创建配置文件

   ![image-20210314164812219](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210314164812219.png)

   ```
   Host github.com
   HostName github.com
   User taozi-jiejie
   PreferredAuthentications publickey
   IdentityFile ~/.ssh/id_rsa_github
   
   Host git.andlinks.com
   HostName git.andlinks.com
   User liyunyan
   PreferredAuthentications publickey
   IdentityFile ~/.ssh/id_rsa
   
   ```

5. 验证是否成功

   ```
   $ ssh -T git@github.com
   Warning: Permanently added the RSA host key for IP address '13.229.188.59' to the list of known hosts.
   Hi taozi-jiejie! You've successfully authenticated, but GitHub does not provide shell access.
   ```

   