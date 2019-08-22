# git常用命令
- 克隆
  - git clone
  
  
- 拉
  - git pull
  
  
- 提交
  - git push
  
  
- 处理冲突合并
   - git stash save 'sjh'
   - git pull
   - git stash pop
   - git push
   
   
- 列出本地已存在分支
    - git branch
    
    
- 列出远程分支
    - git branch -r
    
    
- 列出本地分支和远程分支
    - git branch -a
    - http://blog.csdn.net/wirelessqa/article/details/20153689
        
    
- 查看提交记录(查找commit id)
    - git log
        
    
- 回滚到某个提交ID
    - git reset --hard commit_id
        
    
- 强制提交
    - git push origin master -f