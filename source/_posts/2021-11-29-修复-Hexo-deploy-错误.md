---
title: 修复 Hexo deploy 错误
tags:
  - hexo
cover: 'https://i.loli.net/2021/05/18/JyVe4iDuXAvOjrt.jpg'
abbrlink: 81d3f69e
date: 2021-11-29 18:45:17
---




运行 `hexo d` 时提示错误，按照网上提供的几种方法也无法修复，仍然报错。

```bash
fatal: unable to access 'https://github.com/noviachen/noviachen.github.io.git/': LibreSSL SSL_connect: SSL_ERROR_SYSCALL in connection to github.com:443 

FATAL {

 err: Error: Spawn failed

   at ChildProcess.<anonymous> (/Users/wangwanfang/gitblog/node_modules/hexo-deployer-git/node_modules/hexo-util/lib/spawn.js:51:21)

   at ChildProcess.emit (node:events:365:28)

   at Process.ChildProcess._handle.onexit (node:internal/child_process:290:12) {

  code: 128

 }

} Something's wrong. Maybe you can find the solution here: %s https://hexo.io/docs/troubleshooting.html
```



运行 `ssh -T git@github.com` 提示 `git@github.com: Permission denied (publickey).`，怀疑是 SSH Key 丢失了。



1. 打开终端，输入以下命令

   ```bash
   ssh-keygen
   
   #以下为执行结果
   Generating public/private rsa key pair.
   Enter file in which to save the key (/Users/rahulwagh/.ssh/id_rsa):
   Enter passphrase (empty for no passphrase): 
   Enter same passphrase again: 
   Your identification has been saved in /Users/rahulwagh/.ssh/id_rsa.
   Your public key has been saved in /Users/rahulwagh/.ssh/id_rsa.pub.
   The key fingerprint is:
   SHA256:Okq3w+SesCGLQVToSBQru8RdUZtT2EIIrzH5MQ67DWA rahulwagh@local
   The key's randomart image is:
   +---[RSA 3072]----+
   |.ooo..+oo.       |
   | oo o..o+.       |
   |=E = = +.        |
   |*oo X o .        |
   |.+ = o  S        |
   |o.  + ..         |
   |o ..+=+          |
   | o + *++         |
   |. . o.+.         |
   +----[SHA256]-----+
   ```

2. 可以在 /Users/your_user_name/.ssh 中找到 `id_rsa.pub`，并复制其中的内容

3. 登录 Github，依次打开 GitHub Account -> Settings -> SSH and GPG keys

4. 点击 SSH keys 右边的 `New SSH key`，输入上面复制的内容，保存即可。![](https://i.loli.net/2021/11/28/YC9KWg4eN6DRPMU.png)

   

   ![ssh-keys-added-to-github](https://i.loli.net/2021/11/28/4trND1konFLQBZ8.jpg)

5. 重新运行 `hexo clean && hexo g && hexo d`，大功告成。



> 参考文章：[How to fix - git@github.com permission denied (publickey). fatal could not read from remote repository](https://jhooq.com/github-permission-denied-publickey/)
