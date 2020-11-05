# GIt Note

## git 保存用户名和密码

Git可以将用户名，密码和仓库链接保存在硬盘中，而不用在每次push的时候都输入密码。

```ruby
$ git config credential.helper store
```

保存密码到硬盘一条命令就可以

```ruby
$ man git | grep -C 5 password
$ man git-credential-store
```

作者：有阿爱
链接：https://www.jianshu.com/p/47a51aeeea62
来源：简书
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。