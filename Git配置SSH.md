# Git配置SSH

## 安装相关软件包

```
pacman -S git
```

## 创建SSH密钥

```
ssh-keygen -t rsa -C "xxx@xxx.xx"
```

查看刚刚创建的公钥

```
cat id_rsa.pub
```

如下为例

```
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQDb1tL8JeyOIOKeA2/iYFrYGyN0R61O10dH3Y+a/RXFxYtRiDqmRqOMxA+SrwDEKxdfWHbwtQ/SPi+WxDFrs672Afb2Djlz/Z2IqEPjE9HmXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXT20OAgqZtdC2t4L/XjffOCrXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX56RrYp4BYM5gQA7fElOVIb5F/SdMk107YafaBXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX2SNyjAUssEDUPUoowOm/Vngt5+YyXy8L49WTOQ+SqUqNm+BpOVEQjTNuoMgoKbP/oR0SJwLh0ujMdRf9Gl4kLwx6zWRgLrZU/nNa2i2mL1T5+hfpJKuNHHup8slbF+KzGLguC+m9si91APLWPYGUsAKKha4M5aKezpH5eH36udP8vuDUexEJdWorDfBfwsGi9ICa0TBOdTR9i44+binpT+BdykNP90= xxx@xx.xx
```

## 添加到GitHub

登录到GitHub账号，点击头像 - `Settings` - `SSH and GPG keys` - `New SSH key` - 复制进去，大功告成~

## 测试连接

```
ssh -T git@github.com
```

看到如下信息，就说明配置成功了

```
Hi XXX! You've successfully authenticated, but GitHub does not provide shell access.
```