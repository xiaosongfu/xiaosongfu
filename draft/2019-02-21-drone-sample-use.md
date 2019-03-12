## Steps 

https://docs.drone.io/user-guide/pipeline/steps/

`steps` 部分定义了要顺序执行的步骤的集合。

执行每一步都会启动一个容器，这个容器里面包含项目的 clone，然后执行在 `commands` 部分定义的命令。

这些命令都是在 git 仓库的根目录下执行。git 仓库的根目录也称为 `workspace`，它是一个在你的 pipeline 中的所有 step 都共享的挂载卷（mounted volume）。

容器的 exit code 判断当前 step 是成功还是失败。如果一个 step 返回非0退出码，该步骤会被标记为失败。并且这个 pipeline 都会被标记为失败，剩下的 pipeline steps 都会被跳过。

```
steps:
- name: build
- image: golang
- commands:
    - go build
    - go test
```

#### 