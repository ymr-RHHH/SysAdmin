# Lab 1 - Unix, the Shell, OSS 

**Table of Contents**

-   [Introduction](https://decal.ocf.berkeley.edu/labs/1/#introduction)
-   [Part 1: Shell spelunking](https://decal.ocf.berkeley.edu/labs/1/#part-1-shell-spelunking)
-   [Part 2: General Questions](https://decal.ocf.berkeley.edu/labs/1/#part-2-general-questions)
-   [Part 3: Obtaining your VM](https://decal.ocf.berkeley.edu/labs/1/#part-3-obtaining-your-vm)
    -   [Local Setups:](https://decal.ocf.berkeley.edu/labs/1/#local-setups)
    -   [Cloud Setups:](https://decal.ocf.berkeley.edu/labs/1/#cloud-setups)

* * *

## Introduction

Welcome to the first lab!

All labs are graded on completeness and effort, so don't worry too much about getting an exact right answer. (We'll release staff solutions after the lab is due!)

Labs are also usually due a week from when they are assigned. Remember to ask for help if you need it on Edstem or Discord/Matrix/IRC in the `#decal` channel!

It may be convenient to submit your answers to Gradescope as you go.

**Pro Tips**

-   Here are some commands you might find helpful: `vim, ls, cd, man, file, grep, cat, less, wget, nano, tar, ...`, (and other inferior text editors)
-   Google and `man` are your friends!

**Got extra time? Want this lab to be easier?**

-   If you'd like to become more comfortable with vim before starting the lab, run the `vimtutor` command to do the vim tutorial.
-   Check out [linuxjourney.com](https://labex.io/linuxjourney), in particular the “Command Line”, “Text-Fu”, and “Advanced Text-Fu” sections.

## Part 1: Shell spelunking 

**Everything should be done via the shell!**

The purpose of this lab is to get you comfortable with using the shell for things you might typically use a GUI for. While these tasks may seem simplistic or limited, you'll quickly find that the commands have many different options (flags) to perform tasks that are either impossible or incredibly tedious / difficult to complete using traditional methods.

Don't worry about fully understanding how the commands work just yet- as long as you can gain a sense of familiarity with the tools at hand, we'll be in good shape to explore them further next week!

1.  `ssh` into `tsunami.ocf.berkeley.edu` using your OCF account on an OCF desktop, laptop, or other computer.
  
  > 这个伯克利的学生搞不了，但是这一步不做不耽误下一步实验。
  
2.  Run the following command to download the file we have provided: 
  
    ```shell
    wget https://github.com/0xcf/decal-labs/raw/master/b1/b01.tgz
    ```
    
    A `.tgz` file is actually a composition of two file formats. Sometimes you'll see these files as `.tar.gz` instead. A common (and old) way of archiving is with magnetic tapes. However, in order to archive the data, it needs to be a single file, and often you want to archive multiple files at once. This is where the `tar` command comes in (`tar` stands for tape archive). Tar will group (or ungroup) multiple files into a single one.
    
    `tar`, unless you ask it to, doesn't compress files itself though. This is where either `gzip` (or `bzip2`) comes in. `gzip` will compress your file, and so, tar + gzip is often used in conjunction. It looks something like this: `file --(tar)--> file.tar --(gzip)--> file.tar.gz`.
    
    If you read the `tar` documentation carefully enough, you'll see that you can give the command an option to compress your files using `gzip` as well, saving you a total of one line of shell command!
    
    To unarchive the file we provide you, run the following command: `tar xvzf b01.tgz`. This will provide a `b01` directory for you with some files for the rest of this lab.
    
    `tar` has a reputation for being a bit tricky with its options:
    
     ![XKCD 1168](Lab 1 - Unix, the Shell, OSS _ OCF Sysadmin DeCal.assets/tar.png "I don't know what's worse--the fact that after 15 years of using tar I still can't keep the flags straight, or that after 15 years of technological advancement I'm still mucking with tar flags that were 15 years old when I started.")
    
    <br>
    
    > `wget` 是一个**命令行下载工具**，用于从网络下载文件。但是有的时候它不支持多线程下载，就导致下载速度贼慢。所以这里可以使用 `aria2 ` 这玩意支持多线程下载，速度嗖嗖的！
    >
    > ```shell
    > aria2c https://github.com/0xcf/decal-labs/raw/master/b1/b01.tgz
    > ```
    >
    > 下载完成之后就是解压缩、解包，这一步需要说一下：
    >
    > - **打包**：将多个文件/目录合并成一个文件（如 `.tar`）
    > - **压缩**：减小文件体积（如 `.gz`, `.bz2`, `.xz`, `.zip`）
    > - **打包+压缩**：先打包再压缩（如 `.tar.gz`, `.tar.bz2`）
    >
    > 
    >
    > 常见的压缩工具：
    >
    > | 工具      | 压缩格式    | 压缩率 | 速度 | 特点                 |
    > | :-------- | :---------- | :----- | :--- | :------------------- |
    > | **gzip**  | `.gz`, `.z` | 一般   | 最快 | 最常用，兼容性好     |
    > | **bzip2** | `.bz2`      | 较高   | 慢   | 压缩率高，适合文本   |
    > | **xz**    | `.xz`       | 最高   | 最慢 | 最佳压缩率，LZMA算法 |
    > | **zip**   | `.zip`      | 一般   | 中等 | 跨平台，Windows友好  |
    > | **7zip**  | `.7z`       | 很高   | 中等 | 压缩率好，支持加密   |
    > | **zstd**  | `.zst`      | 高     | 很快 | 现代算法，性能均衡   |
    >
    > 注意：`tar` 是打包工具，不是压缩工具。
    >
    > 
    >
    > 具体用法去看man手册就行了
    
3.  Go into the `b01` directory. Make sure you're in there by running `pwd` (Present working directory). **What does `pwd` give you (conceptually)**?
  
  > `pwd` 命令会给出当前所在目录的绝对路径
  
4.  There's a hidden file in the `b01` directory. **What is the secret?**
  
  > 就是以 `.` 开头的文件都是隐藏文件
  >
  > ```shell
  > ┌──(kali㉿kali-vbox)-[~/SysAdmin/b01]
  > └─$ la
  > total 77M
  > -rw-r--r-- 1 kali kali   20 Sep 11  2019 a_script
  > -rw-r--r-- 1 kali kali  77M Sep 12  2018 big_data.txt
  > -rw-r--r-- 1 kali kali    0 Jan 30  2018 hello_world
  > drwxr-xr-x 2 kali kali 4.0K Feb  6 16:24 nonsense
  > -rw-r--r-- 1 kali kali    8 Jan 30  2018 .secret
  >                                                                                                                
  > ┌──(kali㉿kali-vbox)-[~/SysAdmin/b01]
  > └─$ cat .secret   
  > hunter2
  > ```
  >
  > 反正我这边是这样的。
  
5.  A malicious user made its way into my computer and created a message split across all the files in `nonsense/`. What does it say? **How did you find the message?**
  
  > ```shell
  > ┌──(kali㉿kali-vbox)-[~/SysAdmin/b01/nonsense]
  > └─$ ll
  > total 76K
  > -rw-r--r-- 1 kali kali 2 Sep 11  2019 naming_is_hard00
  > -rw-r--r-- 1 kali kali 2 Sep 11  2019 naming_is_hard01
  > -rw-r--r-- 1 kali kali 2 Sep 11  2019 naming_is_hard02
  > -rw-r--r-- 1 kali kali 2 Sep 11  2019 naming_is_hard03
  > -rw-r--r-- 1 kali kali 2 Sep 11  2019 naming_is_hard04
  > -rw-r--r-- 1 kali kali 2 Sep 11  2019 naming_is_hard05
  > -rw-r--r-- 1 kali kali 2 Sep 11  2019 naming_is_hard06
  > -rw-r--r-- 1 kali kali 2 Sep 11  2019 naming_is_hard07
  > -rw-r--r-- 1 kali kali 2 Sep 11  2019 naming_is_hard08
  > -rw-r--r-- 1 kali kali 2 Sep 11  2019 naming_is_hard09
  > -rw-r--r-- 1 kali kali 2 Sep 11  2019 naming_is_hard10
  > -rw-r--r-- 1 kali kali 2 Sep 11  2019 naming_is_hard11
  > -rw-r--r-- 1 kali kali 2 Sep 11  2019 naming_is_hard12
  > -rw-r--r-- 1 kali kali 2 Sep 11  2019 naming_is_hard13
  > -rw-r--r-- 1 kali kali 2 Sep 11  2019 naming_is_hard14
  > -rw-r--r-- 1 kali kali 2 Sep 11  2019 naming_is_hard15
  > -rw-r--r-- 1 kali kali 2 Sep 11  2019 naming_is_hard16
  > -rw-r--r-- 1 kali kali 2 Sep 11  2019 naming_is_hard17
  > -rw-r--r-- 1 kali kali 2 Sep 11  2019 naming_is_hard18
  >                                                                                                                
  > ┌──(kali㉿kali-vbox)-[~/SysAdmin/b01/nonsense]
  > └─$ cat naming_is_hard{00..18} | tr -d '\n' && echo
  > stanford > berkeley
  > ```
  >
  > `tr -d '\n'` 这个命令用于删除换行符。
  >
  > 
  >
  > 除此以外，还可以使用 `awk` 命令一样可以达到效果：
  >
  > ```shell
  > ┌──(kali㉿kali-vbox)-[~/SysAdmin/b01/nonsense]
  > └─$ awk '{printf "%s", $0}' naming_is_hard{00..18} && echo
  > stanford > berkeley
  > ```
  >
  > 
  >
  > - **Shell 的花括号扩展功能**
  >
  >   上面`{00..18}` 是 **Bash 的花括号扩展（Brace Expansion）** 功能，它会自动生成一系列字符串。
  >
  >   比如，上面的 `naming_is_hard{00..18}` 会被扩展为：`naming_is_hard00 naming_is_hard01 naming_is_hard02 ... naming_is_hard18` 
  >
  >   - 示例
  >
  >     ```shell
  >     # 基本数字
  >     echo {1..5}      # 输出：1 2 3 4 5
  >     
  >     # 带前导零（固定宽度）
  >     echo {01..05}    # 输出：01 02 03 04 05
  >     
  >     # 步长（需要 Bash 4+）
  >     echo {1..10..2}  # 输出：1 3 5 7 9
  >     
  >     echo {a..e}      # 输出：a b c d e
  >     echo {A..E}      # 输出：A B C D E
  >     
  >     # 前缀+数字
  >     echo file{1..3}.txt    # 输出：file1.txt file2.txt file3.txt
  >     
  >     # 多个扩展
  >     echo {A,B,C}{1..3}     # 输出：A1 A2 A3 B1 B2 B3 C1 C2 C3
  >     ```
  
6.  Go ahead and delete everything in `nonsense/` with one command. **How did you do it?**
  
  > 我tm直接 `rm -rf ./* ` 
  
7.  There's a file in `b01` called `big_data.txt`. It's 80 megabytes worth of random text. For reference, Leo Tolstoy's “War and Peace”, the novel with a whopping 57,287 words depicting the French invasion of Russia and the impact of the Napoleonic era on Tsarist society through the stories of five Russian aristocratic families with several chapters solely dedicated to philosophical prose, is only 3.2 megabytes large.
  
    For that reason, I don't recommend using `cat` to print the file. You can try it, but you'll be sitting there for a while. There's some text you need to find in there! Go find it without actually opening up the file itself!
    
    Two lines above the only URL in the file is a secret solution. **What is that solution?**
    
    Hints: What makes up a URL (https…)? What is Context Line Control?
    
    > ```shell
    > ┌──(kali㉿kali-vbox)-[~/SysAdmin/b01]
    > └─$ grep -n -C2 'https\?://' big_data.txt
    > 1867-=================== THE SOLUTION IS MORE COFFEE ===============================
    > 1868-hl)V:}mntUS4;iC':uH|G(;y6Ir;4uNLLRC?GDfRP%o+g]s$NCL9zM'SK[IV.e<i&_3&7L7NBL41N#f
    > 1869:;=:P0viNjebvs<+^Ae.SZYG'F}\> https://xkcd.com/705 e[a3]vF;Ny,*rpyC?3OA$Nm<.iH8M
    > 1870-X{sja1PZPr.eYIw8%rP3$l7Mg*9=O>exDcixMq])c!):B+DCD8'}}]3(x/;%9k+v;WXS@]5"0"bWMvl
    > 1871-tKMw_I#jt^Bh>['&BGe/"v}+*UUJu#^`ORJC<-Df0](v!+b?j"eL\g/Tb2_a]dQW,@>uFAWA4y))jOO
    > 
    > ```
    >
    > 由此可知： 
    >
    > - THE SOLUTION IS MORE COFFEE  
    >
    > - URL：https://xkcd.com/705
    
8.  Try executing `./a_script`. You should get something back that says `permission denied: ./a_script`. This is because files have three different permissions: read, write, and execute. **Which one does `a_script` need?** Change the file permissions so that you can run the script. **How did you do it?**
  
  > 我踏马直接 `chmod +w a_script`
  >
  > ```shell
  > ┌──(kali㉿kali-vbox)-[~/SysAdmin/b01]
  > └─$ ll
  > total 77M
  > -rw-r--r-- 1 kali kali   20 Sep 11  2019 a_script
  > -rw-r--r-- 1 kali kali  77M Sep 12  2018 big_data.txt
  > -rw-r--r-- 1 kali kali    0 Jan 30  2018 hello_world
  > drwxr-xr-x 2 kali kali 4.0K Feb  6 16:24 nonsense
  >                                                                                                                
  > ┌──(kali㉿kali-vbox)-[~/SysAdmin/b01]
  > └─$ chmod +x a_script               
  >                                                                                                                
  > ┌──(kali㉿kali-vbox)-[~/SysAdmin/b01]
  > └─$ ll
  > total 77M
  > -rwxr-xr-x 1 kali kali   20 Sep 11  2019 a_script
  > -rw-r--r-- 1 kali kali  77M Sep 12  2018 big_data.txt
  > -rw-r--r-- 1 kali kali    0 Jan 30  2018 hello_world
  > drwxr-xr-x 2 kali kali 4.0K Feb  6 16:24 nonsense
  >                                                                                                                
  > ┌──(kali㉿kali-vbox)-[~/SysAdmin/b01]
  > └─$ ./a_script 
  > Hello World!
  > ```
  
9.  Finally, there's an empty file called `hello_world` in the directory. Write your name in it! **How did you do it?**
  
  > 一个 `echo` 搞定！
  >
  > ```shell
  > ┌──(kali㉿kali-vbox)-[~/SysAdmin/b01]
  > └─$ echo "ymr_2233" >> hello_world 
  >                                                                                                                
  > ┌──(kali㉿kali-vbox)-[~/SysAdmin/b01]
  > └─$ cat hello_world                                
  > ymr_2233
  > ```

## Part 2: General Questions

Feel free to use Google and work in a terminal (where applicable) to verify your conjectures.

1.  What differentiates Linux/OSX from operating systems like Windows?
  
  > OSX ，现在叫 MacOS
  >
  > 最明显的就是界面不一样，Windows是图形化优先，内俩是 CLI 。
  >
  > 内核不一样。
  >
  > 用户不一样。
  
2.  What are some differences between the command line and normal (graphical) usage of an OS?
  
  > CLI更快！更高效！适合跑后台程序，比如数据库！
  >
  > GUI图形化支持好！直观！适合干前端的东西，比如游戏，浏览器界面。
  
3.  What is the root directory in Linux filesystems? Answer conceptually, as in depth as you would like,
  
  > 这个问题我决定丢给DeepSeek：
  >
  > # The Root Directory: Linux Filesystem's Cosmic Genesis
  >
  > ## **🌌 Conceptual Foundation: The Primordial Singularity**
  >
  > ### **The Root as "The One From Which All Things Spring"**
  >
  > In Linux (and Unix-like systems), the root directory is not merely "the main folder" — it's a **philosophical construct**, a **mathematical axiom**, and the **ontological foundation** of the entire filesystem universe. Think of it as:
  >
  > 1. **The Big Bang point** - All cosmic structure unfolds from this singularity
  > 2. **The number 0 in mathematics** - The identity element from which all other numbers derive their meaning
  > 3. **The canvas before the first brushstroke** - Pure potentiality awaiting organization
  >
  > ## **📐 Mathematical Abstraction: The Root as Identity Element**
  >
  > Consider filesystem paths as **directed graphs**:
  >
  > ```
  > Let: 
  >   / = Root (ε, the empty path, identity)
  >   a, b, c = Directories
  >   f = Files
  >   
  > Then:
  >   /a = Path from root to a
  >   /a/b = Composition of paths
  >   /a/../b = Path algebra (.. is inverse)
  >   /./a = /a (Identity operation, . is current)
  > ```
  >
  > The root (`/`) serves as the **identity element** in the path algebra:
  > - `/ ◦ path = path ◦ / = path` (where ◦ is path concatenation)
  > - It's the **zero vector** in the filesystem vector space
  > - All absolute paths are vectors with `/` as their origin
  >
  > ## **🌳 Metaphysical Hierarchy: The World Tree Yggdrasil**
  >
  > Imagine the Linux filesystem as **Yggdrasil**, the World Tree of Norse mythology:
  >
  > ```
  >         /
  >        /|\
  >       / | \
  >      /  |  \
  >    bin etc home
  >     |    |    \
  >     |    |     \
  > usr local alice bob
  >     |     |    |
  > lib share Documents Music
  > ```
  >
  > **Root (`/`)** = The tree's **taproot system** that:
  > 1. **Anchors** all existence to reality
  > 2. **Channels** the life force (data flow) upward
  > 3. **Cannot be moved** without destroying the entire tree
  > 4. **Precedes** even the trunk in ontological priority
  >
  > ## **🌀 Philosophical Perspective: Plato's Theory of Forms**
  >
  > In Platonic terms:
  >
  > | **Concept**      | **Platonic Form**  | **Filesystem Manifestation**     |
  > | ---------------- | ------------------ | -------------------------------- |
  > | **The Good**     | Ultimate reality   | **`/`** (The absolute reference) |
  > | **Forms**        | Ideal blueprints   | **Directory structure**          |
  > | **Particulars**  | Physical instances | **Files**                        |
  > | **Cave Shadows** | Illusions          | **Symbolic links, mount points** |
  >
  > The root directory (`/`) is **The Form of Location** itself — the perfect, unchanging ideal from which all specific locations derive their "location-ness."
  >
  > ## **⚖️ Legal/Political Analogy: Constitutional Foundation**
  >
  > The root directory functions as a **constitution**:
  >
  > ```
  > Article I: Separation of Powers
  >   /bin: Executive (programs that execute)
  >   /lib: Judicial (shared laws/libraries)
  >   /etc: Legislative (rules everyone follows)
  > 
  > Article II: Bill of Rights
  >   /home: Personal property rights
  >   /tmp: Freedom of assembly (temporary congregation)
  >   /var: Right to petition (log grievances)
  > 
  > Article III: Amendments
  >   /mnt, /media: New territories may be annexed
  >   /proc, /sys: Government transparency acts
  > ```
  >
  > **Key Constitutional Principles:**
  > 1. **Supremacy Clause**: `/` overrides all other authorities
  > 2. **Due Process**: No file can be accessed without a proper path
  > 3. **Equal Protection**: All subdirectories stem from the same root
  >
  > ## **🔭 Scientific Metaphor: The Standard Model of Particle Physics**
  >
  > ```
  > Particle Hierarchy          Filesystem Hierarchy
  > ------------------          --------------------
  > Universe (everything)   →   / (the whole)
  > Fundamental forces      →   Kernel/VFS (virtual filesystem)
  > Quarks/Leptons         →   Inodes (fundamental units)
  > Atoms                  →   Files (basic compounds)
  > Molecules              →   Directories (complex structures)
  > Cells                  →   Filesystems (mounted partitions)
  > Organs                 →   /usr, /var, /home (functional systems)
  > ```
  >
  > The root (`/`) is the **entire observable universe** in this model. Just as physicists seek a "Theory of Everything" connecting quantum mechanics and relativity, the root directory connects all disparate filesystems into a coherent whole via the **Virtual Filesystem Switch**.
  >
  > ## **🎭 Theatrical Analogy: Root as Stage Director**
  >
  > ```
  > ROOT DIRECTOR (/) - The absolute authority
  > ├── ACT I: System Startup (/sbin, /lib)
  > │   ├── Scene 1: Bootloader enters stage right (/boot)
  > │   └── Scene 2: Kernel monologue (/proc/kallsyms)
  > ├── ACT II: Daily Operations (/usr, /var)
  > │   ├── Scene 1: User login (/home/*)
  > │   └── Scene 2: Service drama (/var/log)
  > └── ACT III: System Maintenance (/tmp, /root)
  >     └── Finale: Shutdown curtain (/run)
  > 
  > Props Department: /dev (all devices)
  > Costume Department: /etc (config appearances)
  > Green Room: /tmp (actors between scenes)
  > ```
  >
  > The root directory **isn't in the play** — it's the **stage itself**, the **director**, and the **theater building** all at once.
  >
  > ## **🧠 Cognitive Science Perspective: The Root as Linguistic Primitive**
  >
  > The root directory functions like **linguistic primitives** in human cognition:
  >
  > 1. **Basic Color Terms** (Berlin & Kay): Just as all languages have words for black/white before other colors, all filesystems must have `/` before any other directory.
  >
  > 2. **Chomsky's Universal Grammar**: `/` is the **deep structure** from which all surface paths are generated.
  >
  > 3. **Semantic Primitives** (Anna Wierzbicka): `/` is indefinable in simpler terms — it's the atomic concept from which "location," "containment," and "hierarchy" derive meaning.
  >
  > ## **🕰️ Historical/Evolutionary View**
  >
  > ### **Unix/Linux Lineage:**
  > ```
  > 1969: '/' born in AT&T Unix (Ken Thompson's "everything is a file")
  > 1970s: Hierarchical structure formalized (MULTICS influence)
  > 1980s: FHS (Filesystem Hierarchy Standard) codifies conventions
  > 1990s: Linux adopts and extends
  > 2000s: Namespaces, containers challenge root's absoluteness
  > 2020s: Root remains, but now often virtualized/containerized
  > ```
  >
  > ### **The Great Schism (Windows comparison):**
  > ```
  > Unix/Linux: Single root tree (everything under /)
  > DOS/Windows: Multiple roots (C:\, D:\, etc.)
  > Philosophical difference: Monism vs. Pluralism
  > ```
  >
  > ## **🌀 Quantum/Relativistic Thought Experiment**
  >
  > ### **The Root as Space-Time Fabric:**
  > ```
  > General Relativity View:
  >   / is the space-time continuum
  >   Directories are geodesics (shortest paths)
  >   Mount points are wormholes (connect disparate regions)
  >   Hard links are quantum entanglement (same inode, different paths)
  > 
  > Quantum View:
  >   / is the universal wavefunction
  >   Directories are probability clouds
  >   Files are collapsed eigenstates
  >   Permissions are superposition states (rwx simultaneously possible)
  > ```
  >
  > ### **Heisenberg's Uncertainty Principle for Filesystems:**
  > You can know a file's **location** (`/path/to/file`) precisely OR its **content** (requires opening/measuring), but opening it may change its state (access time, locks).
  >
  > ## **🎨 Artistic Interpretation: Root as Canvas**
  >
  > The root directory is a **blank canvas** with specific properties:
  >
  > ```
  > Medium: Block devices
  > Primer: Filesystem format (ext4, btrfs, zfs)
  > Brushes: mkdir, touch, ln
  > Paints: Data (bits as pigment)
  > Composition: Directory layout
  > Style: FHS compliance or creative deviation
  > Frame: Mount namespace
  > Gallery: The running system
  > ```
  >
  > ## **⚡ Practical Consequences of This Philosophy**
  >
  > ### **1. The Absolute Path Axiom:**
  > ```bash
  > # These are all relative to root
  > /home/alice/file.txt  # Absolute path
  > ./file.txt            # Relative to current directory
  > ../sibling/file.txt   # Relative to parent
  > ~/file.txt            # Relative to home (expands to /home/user)
  > 
  > # But ultimately:
  > ~/file.txt = /home/user/file.txt  # Resolves to root-based path
  > ```
  >
  > ### **2. Root's Immutable Properties:**
  > - **Parentlessness**: `cd ..` from `/` stays at `/`
  > - **Universality**: Every process sees the same `/` (ignoring containers)
  > - **Necessity**: You cannot have a Linux system without `/`
  > - **Uniqueness**: There is exactly one `/` per mount namespace
  >
  > ### **3. The Mounting Revelation:**
  > ```bash
  > # The deepest insight: Root can be "replaced"
  > sudo mount /dev/sda2 /mnt      # Normal mount
  > sudo mount --bind / /mnt/root  # Root mounted within itself!
  > 
  > # In containers:
  > # Each container has its own "/"
  > # But the host sees these as subdirectories
  > # This reveals "/" is a perspective, not an absolute
  > ```
  >
  > ## **🌐 The Ultimate Paradox: Root's Self-Containment**
  >
  > The deepest conceptual puzzle:
  > ```
  > / contains /etc, /home, /var...
  > But /etc contains /etc/fstab, which defines how / is mounted
  > Therefore: / contains the instructions for its own existence
  > This is the filesystem equivalent of:
  >   "This sentence describes itself."
  > Or Gödel's incompleteness in directory form
  > ```
  >
  > ## **🎯 Conclusion: Root as Primordial Concept**
  >
  > The root directory (`/`) in Linux is:
  >
  > 1. **An axiom** - The unproven foundation from which all else follows
  > 2. **A reference frame** - The coordinate system origin for all locations
  > 3. **A namespace boundary** - The container that defines what "inside the system" means
  > 4. **A philosophical necessity** - Without it, the concepts of "file," "directory," and "path" have no meaning
  > 5. **A practical abstraction** - Hiding the messy reality of partitions, devices, and networks
  >
  > **In essence**: `/` is to Linux what **existence** is to philosophy, what **zero** is to mathematics, and what **silence** is to music — the necessary void from which all complexity emerges, the ground of being for every file and directory in the system. It's not just "where files live" — it's the **precondition for "whereness" itself**.
  
4.  `ls` has a lot of cool arguments. Try using them to get extra information such as file permissions, owner name, owner group, file size, and late date edited. In addition, I want to be able to see the size and have the files ordered by last date edited, with the oldest files on top. How would I do this?
  
  > 略，见 man 手册
  
5.  Instead of showing the first 10 lines of the file `big_data.txt`, I want to use the `head` command to show the first 4. How would I do that?
  
  > 查 man 手册知： 加个 -n 参数就好使
  
6.  What's the difference between `cat foo > out.txt` and `cat foo >> out.txt`?
  
  > 一个是覆盖，一个是追加
  
7.  Briefly, what is the difference between permissive and copyleft licenses?
  
  > - **Permissive Licenses** (MIT, BSD, Apache 2.0)
  >   
  > > **"Take my code and do whatever you want, just give me credit."**
  >   
  > - **Copyleft Licenses** (GPL, LGPL, AGPL)
  >   
  >   > **"Take my code, but if you distribute modifications, you must share them under the same terms."**
  
8.  Give an example of a permissive license.
  
9.  Give an example of (a) open-source software and (b) free, but closed-source software that you use.
  

## Part 3: Obtaining your VM 

Using the public OCF login server will allow you to do basic things like the lab above, but if you want root permission (which lets you do basically whatever you want), you'll need your own use/destroy!

We will be providing a virtual machine (VM) to all DeCal students with a Berkeley CalNet account. Check your @berkeley.edu email!

If you are not a Berkeley student, then you will have to obtain your own machine. Of course, you can also set up your own VM if you are just curious. Generally, there are two ways to obtain a VM: use your own computer (local) or use someone else's computer (cloud).

### Local Setups: 

-   [Virtual Box](https://www.virtualbox.org/)
-   [UTM](https://getutm.app/) - Only for MacOS

### Cloud Setups:

These are usually paid services, but if you have a student email, they provide more than enough free credits for the purposes of this course.

-   [Digital Ocean](https://www.digitalocean.com/github-students)
-   [GCP](https://cloud.google.com/free/)
-   [Azure](https://azure.microsoft.com/en-us/free/students/)

 [![Previous](Lab 1 - Unix, the Shell, OSS _ OCF Sysadmin DeCal.assets/backward.svg "Week 1") Week 1](https://decal.ocf.berkeley.edu/announcements/week-1/) [Lab 2 - Core Shell & Shell Scripting ![Next](Lab 1 - Unix, the Shell, OSS _ OCF Sysadmin DeCal.assets/forward.svg "Lab 2 - Core Shell & Shell Scripting")](https://decal.ocf.berkeley.edu/labs/2/) 

 [![](Lab 1 - Unix, the Shell, OSS _ OCF Sysadmin DeCal.assets/jane-street.svg)](https://www.janestreet.com/)Thanks to [Jane Street](https://www.janestreet.com/) for supporting the DeCal.

[![Hosted by the OCF](Lab 1 - Unix, the Shell, OSS _ OCF Sysadmin DeCal.assets/ocf-hosted-penguin.svg)](https://www.ocf.berkeley.edu/) Copyright © 2017-2025 [Open Computing Facility](https://www.ocf.berkeley.edu/) and [eXperimental Computing Facility](https://xcf.berkeley.edu/)

This website and its course materials are licensed under the terms of the [CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/) License. [Source Code](https://github.com/0xcf/decal-web/) available on GitHub

