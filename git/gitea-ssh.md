# Gitea SSH 直通（与主机共享 SSH 端口）

## 创建用户

### 执行命令 1

```sh
sudo groupadd -g 888 git && adduser --uid 888 --gid 888 --system --shell /bin/bash --gecos "Git Version Control" --disabled-password --home /home/git git

# 输出
Adding system user `git' (UID 888) ...
Adding new user `git' (UID 888) with group `git' ...
Creating home directory `/home/git' ...
```

## 生成密钥

创建密钥并添加到 git 的 authorized_keys

### 执行命令 2

```sh
sudo -u git ssh-keygen -t rsa -b 4096 -C "Gitea Host Key" && echo "$(cat /home/git/.ssh/id_rsa.pub)" >> /home/git/.ssh/authorized_keys

# 输出
Generating public/private rsa key pair.
Enter file in which to save the key (/home/git/.ssh/id_rsa):
Created directory '/home/git/.ssh'.
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /home/git/.ssh/id_rsa.
Your public key has been saved in /home/git/.ssh/id_rsa.pub.
The key fingerprint is:
SHA256:kHsow5CKInZwlzShXQOJ7xs1kBf1RN18j3l8BenFtbw Gitea Host Key
The key's randomart image is:
+---[RSA 4096]----+
|    .==+o..o. ++o|
|   o+++o. o  .oo*|
| .oo.+=    . . B+|
|..oo.. *      + *|
|=. .= + S      E.|
|+ .  = .         |
|      o          |
|     .           |
|                 |
+----[SHA256]-----+
```

### authorized_keys 输出

```
command="/app/gitea/gitea --config=/data/gitea/conf/app.ini serv key-1",no-port-forwarding,no-X11-forwarding,no-agent-forwarding,no-pty,no-user-rc,restrict ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQDHl5p5WKarLiEosiMHpoGZ81ViThnIFMF8fqMCmPbTuPVmaZi13ftprB9Ncjb6zfTBS+R2UqyPDCxuxZutA6s8GYcwH+o4ZD/xmK3+IbMwLtZ9uzjW3GUdhw/XAq6Ojyr89a0VlV2i51ehmNATACXRt8MpkVn0uHQWXn4+lPqYUj7f6d1c3WP+be1roU0w9waByJpcBm0Owgi54KKIzrgy4t0G9di3Gub++TMHT4vBWHKdFLe6bBqR2soET5f6sUXSSPLoAT9c2j+0mpVExyv2btovS9n89blBzqM7K2VxwGbboMRN53PIT0TGJb5Nt4mWWj7GYfFoYSdR14O0cc9Fc0KNHzIqtyM90oL7u3LU6zK9Na75xTTPO9GH2LJ06YurzvVKemRCixjbTuukcpcY1vg5pknpcF2w7WjNItOpl9HZqoM677rhTwMT9yTQWVI7kakvbHer51HnrgSmlvd7Urc+OhPUZgGxPzZkZUpUppHIKcz75aeZTRq7Bq98z18= narcissu456@gmail.com
```

**特别注意**

如果 push 时提示 `bash: line 1: /usr/local/bin/gitea: No such file or directory`

**解决方法**

**检查 authorized_keys 是否 ·command ="/app/gitea/gitea --config ...` 开头。**

## 执行文件

### 执行命令 3

SSH 的直通文件

```sh
sudo mkdir -p /app/gitea && sudo echo 'ssh -p 8092 -o StrictHostKeyChecking=no git@127.0.0.1 "SSH_ORIGINAL_COMMAND=\"$SSH_ORIGINAL_COMMAND\" $0 $@"' >> /app/gitea/gitea && sudo chmod +x /app/gitea/gitea
```

输出

```sh
ssh -p 8092 -o StrictHostKeyChecking=no git@127.0.0.1 "SSH_ORIGINAL_COMMAND=\"$SSH_ORIGINAL_COMMAND\" $0 $@"11
```

## 备注

主机 SSH 端口：22
容器映射端口：8092
用户：git
用户组：git
用户 ID：888
用户组 ID：888

docker-compose 参考配置

```yaml
  gitea:
    image: gitea/gitea:1
    container_name: app-gitea-1
    restart: unless-stopped
    environment:
      - USER_UID=888
      - USER_GID=888
      - DB_TYPE=postgres
      - DB_HOST=app-postgresql-1
      - DB_PORT=5432
      - DB_NAME=gitea
      - DB_USER=postgres
      - DB_PASSWD=db_password
    volumes:
      - /app/gitea/data:/data
      - /home/git/.ssh/:/data/git/.ssh
      - /etc/timezone:/etc/timezone:ro
      - /etc/localtime:/etc/localtime:ro
    ports:
      - "8091:3000"
      - "8092:22"
    networks:
      - app_backend
    mem_limit: 512m
```
