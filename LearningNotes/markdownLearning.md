

# Markdown 学习笔记

记一些排版的注意点和一些好玩的小技巧

记录语法没有意义

 

## 关于排版



## 好玩的东西

- flow流程图的使用

```flow
st=>start: 开始
op=>operation: My Operation
cond=>condition: Yes or No?
e=>end
st->op->cond
cond(yes)->e
cond(no)->op
```

```flow
st=>start: index
op=>operation: 申请
op2=>operation: 结果页
op3=>operation: 查询本地
i1=>inputoutput: bid入库
i2=>inputoutput: 填写个人信息
c1=>condition: 检查登录
c2=>condition: 登录
c3=>condition: 查询本地记录
c4=>condition: 检测状态
c5=>operation: 风控审核
e=>end

st->op->c1()
c1(no)->c2(yes)->op()
c1(yes)->c3(no)->i1(right)->i2(right)->c5()->op2->e
c1(yes)->c3(yes)->c4(no)->i2
c1(yes)->c3(yes)->c4(yes)->op3->op2
c3()->e
```



- checkbox的使用

- [x] fdsfa
- [ ] fsdfas



- 锚点的使用

  只能链接到标题, 适合自己做目录, 但我一般用toc语句

  [01](#rrr)

  jflsd
  d
  fds
  fd

  sdf
  dsf
  ds
  fds
  fds
  fds
  f
  dsf
  dsf

  ### rrr

