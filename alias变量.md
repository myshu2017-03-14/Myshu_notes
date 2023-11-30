# 常用
```
# alias
alias ll='ls -l'
alias ls='ls --color=auto'
alias l='ll -ht'
alias le='less -SN'
alias vi='vim'
alias which='alias | /usr/bin/which --tty-only --read-alias --show-dot --show-tilde'
# 查看文件n-m行
lookrow ()
{
  sed -n "$2,${3}p" $1
}
```
# taskmanager
``` shell
# 查看用户所有运行的项目状态
alias tm='/analyse/Software/miniconda3/bin/taskmanager manage stat'
# 查看指定project的所有任务状态
alias tm3='/analyse/Software/miniconda3/bin/taskmanager project logdump --project '
# 清理错误任务
alias tclear='/analyse/Software/miniconda3/bin/taskmanager project clear --project '
# 删除项目
alias tremove='/analyse/Software/miniconda3/bin/taskmanager manage remove_project --project '
# 列出所有整体运行状态和路径
alias display='/analyse/Software/miniconda3/bin/taskmanager manage display'
# 查看具体的任务信息
tjobinfo ()
{
  /analyse/Software/miniconda3/bin/taskmanager manage jobinfo --project $1 --jobpath $2
}

# 修改其中一个任务，其关联的全部重投操作
## 先暂停项目
#/ehpc/Software/miniconda3/bin/taskmanager project modify  --status "Paused" --project main_US_Mus103_demo_test
#/ehpc/Software/miniconda3/bin/taskmanager project modify  --status "Running" --project main_US_Mus103_demo_test
# 然后列出所有需要重投的修改状态Paused/Suspend/Running
#ls /ehpc/project/shumingyue/result_new/RNAVelocity/shell/*_RNAVelocity.sh | xargs -i /ehpc/Software/miniconda3/bin/taskmanager job --status Finished --project main_US_Mus103_demo_test --jobpath {}

```

# 计算.o文件中的时间
```
alias o_time="bash /home/shumingyue/0-myshu_pro/cal_sh_o_file_time.sh"
```

# 删除目录下面测试的shell脚本.o .e等文件
```
rm_eo ()
{
  # 帮助信息
  if [ -z "$1" ];then
    cat <<HELP
---------------------------------------------------------------
  Author: shumingyue
  Mail: shumingyue@genomics.cn
  Version: 1.0
  Date: 2023-11-10
  Description: 删除指定id的qsub测试生成的.e .o
---------------------------------------------------------------
USAGE:
    or  -h # show this message
EXAMPLE:
     -a test.sh "2350939 2351273"  # 删除指定的所有的test.sh.*2350939和test.sh.*2351273
     -a test.sh                    # 删除指定的所有的test.sh.*
     -v test.sh "2350939 2351273"  # 删除除了test.sh.*2350939和test.sh.*2351273之外的其他test.sh.*

HELP
        return 0
  fi
  # 删除指定脚本的所有id的 .o .e文件
  if [ $1 == "-a" ];then
    if [ -n "$3" ];then
      rm_ids=`echo $3 | sed "s/^/*/g;s/ / */g"`
      rm $rm_ids
    else
      rm $2.*
    fi
        return 1
  fi
  # 排除指定的id ，删除脚本其他的id
  if [ $1 == "-v" ];then
    save_ids=`echo $3 | sed "s/ /|/g"`
    ls $2.* | grep -vE "$save_ids" | xargs rm
        return 1
  fi
}
```
