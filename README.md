### ビルド実行手順

1. コマンドを実行するディレクトリを確認

```
$ Asp.netCoreDcokerDev % tree -N -L 2
.
├── README.md
├── TestContainerEnv
│   ├── Controllers
│   ├── Dockerfile
│   ├── Models
│   ├── Program.cs
│   ├── Properties
│   ├── Startup.cs
│   ├── TestContainerEnv.csproj
│   ├── TestContainerEnv.csproj.user
│   ├── Views
│   ├── appsettings.Development.json
│   ├── appsettings.json
│   ├── bin
│   ├── obj
│   └── wwwroot
└── TestContainerEnv.sln

8 directories, 9 files
```

2. コンテナイメージを生成

```
docker build -t aspcore-5-for-docker -f TestContainerEnv/Dockerfile .
```

3. コンテナイメージが生成されていることを確認

```
$ docker images |fgrep "aspcore-5-for-docker"
aspcore-5-for-docker                 latest                                                  35ff4d2f456b   About a minute ago   210MB
```

4. 生成したコンテナイメージを使用して、アプリケーションを起動（8000→80にポートフォワーディング）

```
docker run -it --name cs-for-docker -p 8000:80 aspcore-5-for-docker
```

5. コンテナ上のアプリケーションが起動していることを確認

```
$ docker ps --format "table {{.Names}}: {{.Status}}: {{.Ports}}"
NAMES: STATUS: PORTS
cs-for-docker: Up 3 minutes: 443/tcp, 0.0.0.0:8000->80/tcp
```

6. ブラウザ確認

```
http://localhost:8000/
```

---

7. コンテナを停止

```
docker stop cs-for-docker
```

8. 生成したコンテナイメージを削除（削除対象のイメージを確認）

```
$ docker ps -a
CONTAINER ID   IMAGE                  COMMAND                  CREATED         STATUS                          PORTS     NAMES
e250d1d99097   aspcore-5-for-docker   "dotnet TestContaine…"   6 minutes ago   Exited (0) About a minute ago             cs-for-docker
```

9. 生成したコンテナイメージを削除（イメージIDを指定して削除）

```
docker rm e250d1d99097 && slepp 1s && docker image ls |fgrep e250d1d99097
```
