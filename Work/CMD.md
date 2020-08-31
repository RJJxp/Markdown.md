# cmd

- 显示文件大小

  > ls -lh

- 查看当前目录文件文件夹数量

  ```bash
  # 查看当前目录下文件数量, 不包含子目录中文件
  ls -l|grep "^-"| wc -l
  # 查看当前目录下文件数量, 包含子目录文件
  ls -lR|grep "^-"| wc -l
  # 查看当前目录下文件夹数量, 不包含子目录中文件
  ls -l|grep "^d"| wc -l
  # 查看当前目录下文件夹数量, 包含子目录中文件
  ls -lR|grep "^d"| wc -l
  ```

- ssh 传输文件

  ```bash
  # 复制文件, 服务器到本地
  scp <server_name>@<server_ip>:<server_path> <local_dir>
  # 复制文件夹, 服务器到本地
  scp -r <server_name>@<server_ip>:<server_dir> <local_dir>
  # 复制文件, 本地到服务器
  scp <local_path> <server_name>@<server_ip>:<server_dir> 
  # 复制文件夹, 本地到服务器
  scp -r <local_dir> <server_name>@<server_ip>:<server_dir>
  # 指定服务器端口
  scp -P <port_id> <local_path> <server_name>@<server_ip>:<server_dir>
  ```

- 查看显卡和状态

  ```bash
  # 查看状态, 相当于win10的任务管理器
  htop
  # 查看显卡状态
  nvidia-smi 
  # 实时显示
  nvidia-smi -l
  ```
  
- 文件夹大小

  ```bash
  cd <dst_dir>
  du -h --max-depth=0
  du -h --max-depth=1
  ```
  
- 查看磁盘空间

  ```bash
  df -hl
  ```

- 代理

  ```bash
  sudo apt -o Acquire::http::proxy=http://127.0.0.1:8888 update
  export http_proxy="http://127.0.0.1:8888"
  ```

- youtube-dl

  ```bash
  youtube-dl --proxy 'socks5://127.0.0.1:1089' <url>
  ```

  