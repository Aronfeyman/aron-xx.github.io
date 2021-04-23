常见问题
通过hexo g -d部署时报Error: Spawn failed错误:
这是由于git本地记录的提交版本号与github上不一致导致的，通过git reset --hard commitCode即可解决。

检查本地最近提交记录，获取最后一次提交记录的更新时间及标识，如280a7fdd46fcfd7d34e652aec15523dcd247fac8bash cd .deploy_git cat .git/logs/HEAD
获取github pages服务所关联分支的最近一次提交记录，获取更新时间及标识。地址一般为：https://github.com/用户名/仓库名/commits/分支名，如https://github.com/lxl80/blog/commits/gh-pages
如果发现提交最新的提交时间/标识不一致，通过以下命令即可解决: bash git reset --hard f085038efdf79546c09641d37b2a2429c1ae8e60 #github上最新的提交标识