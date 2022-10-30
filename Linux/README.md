> Linux

## linux下运行npm install 报错

npm 出于安全考虑不支持以 root 用户运行，即使你用 root 用户身份运行了，npm 会自动转成一个叫 nobody 的用户来运行，而这个用户几乎没有任何权限。这样的话如果你脚本里有一些需要权限的操作，比如写文件（尤其是写 /root/.node-gyp），就会崩掉了。为了避免这种情况，要么按照 npm 的规矩来，专门建一个用于运行 npm 的高权限用户；要么加 --unsafe-perm 参数，这样就不会切换到 nobody 上，运行时是哪个用户就是哪个用户，即使是 root。

所以需要使用以下命令来运行

```bash
    npm install --unsafe-perm=true --allow-root
```



