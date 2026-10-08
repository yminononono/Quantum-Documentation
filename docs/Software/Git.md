
## SSH の RSA key の変更
---
default では id_rsa を利用するため、ユーザーごとに rsa key が異なる場合には以下のように変更しないと、clone や push などができない。
```bash
git config core.sshCommand "ssh -i ~/.ssh/id_rsa_qcs"
```

## Jupyter notebook の管理
---

```bash
$ nbstripout --install --attributes .gitattributes
$ git rm --cached *.ipynb # すでに commit 済みの場合、一度 cache を取り除いてから反映が必要
$ git add .

nbdime config-git --enable # jupyter notebook に対する git diff の挙動を変更
```