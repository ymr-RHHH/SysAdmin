GitHub地址：https://github.com/tmux/tmux/

官方教程：[Getting Started · tmux/tmux Wiki](https://github.com/tmux/tmux/wiki/Getting-Started)

快速入门视频：[tmux 使用和基础配置 从入门到加班，一个视频全搞定！]( https://www.bilibili.com/video/BV1Pnche9EwY/?share_source=copy_web&vd_source=dd23e37f34be8abeabaf405e07d31ab0)



# 调整 tmux pane 大小

## **方法1：快捷键方式（最常用）**

先按 `Ctrl+b`，然后松开，再按方向键：

| 操作                    | 快捷键                       |
| ----------------------- | ---------------------------- |
| **调整大小（逐行/列）** | `Ctrl+b` + `↑/↓/←/→`         |
| **调整大小（加速版）**  | `Ctrl+b` + `Alt` + `↑/↓/←/→` |
| **最大化/还原当前pane** | `Ctrl+b` + `z`               |

**注意**：`Ctrl+b` 和 `Alt` 可以同时按住，然后按方向键，这样调整更快。

## **方法2：命令行方式（更精确）**
```bash
# 调整当前pane向上扩大10行
tmux resize-pane -U 10

# 调整当前pane向下缩小5行
tmux resize-pane -D 5

# 向左扩大5列
tmux resize-pane -L 5

# 向右缩小10列
tmux resize-pane -R 10

# 指定某个pane调整（-t 指定目标）
tmux resize-pane -t 2 -U 10  # 调整编号2的pane向上扩大10行
```

## **方法3：鼠标拖拽（如果开启鼠标模式）**
如果启用了鼠标：
```bash
# 在 tmux 配置文件 ~/.tmux.conf 中添加
set -g mouse on
```
然后就可以直接用鼠标拖动 pane 的分割线来调整大小。

## **常用技巧**

- **快速调整**：`Ctrl+b` + `方向键`（一下一下按）
- **大幅调整**：`Ctrl+b` + `Alt` + `方向键`（持续按住方向键）
- **临时全屏**：`Ctrl+b` + `z`（再按一次恢复）









