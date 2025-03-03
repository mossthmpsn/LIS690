# Project Management
- gcloud used to create virtual machine instance of Ubuntu Linux
- GitHub used to document work
- attention to detail important
# Using gcloud for VMs
- virtual machine = virtual OS that runs on a host OS
- create project on google cloud
- installing gcloud CLI
  - download tar file
  - initialize using "gcloud init"
  - click compute engine -> VM instances
  - create instance
  - machine configuration: E2
  - machine type: e2-micro (2 vCPU 1 core 1 GB memory)
  - OS and storage -> CHANGE, select Ubuntu
  - boot disk type: balanced persistent disk, 10 GB disk size
  - allow HTTP traffic
- I will be using the SSH
- update Ubuntu command:
  sudo apt update
  sudo apt -y upgrade
  sudo apt -y autoremove
  sudo apt clean
- "exit" to disconnect
---
# Learning the CLI
- two major interfaces: graphical user interface (GUI) (non-textual interaction i.e. mouse, remote controls, etc) and command line interface (CLI)
- CLI: interactions are all text-based, require more specificity, and can be automated
- basic commands:
  list files and directories.................. ls
  print name of current/working directory..... pwd
  create a new directory...................... mkdir
  remove or delete an empty directory......... rmdir
  change directory............................ cd
  create an empty file........................ touch
  print characters to output.................. echo
  display contents of a text file............. cat
  copy a file or directory.................... cp
  move or rename a file or directory.......... mv
  remove or delete a file or directory........ rm
- filesystem
  - in Linux: where the directories on the system are placed
  - mac and linux both have a root location (/home/USER)
- Bash
  - "Bourne Again Shell"
  - shell: interactive command language and scripting language
---
# Text editors
- plain text: most basic way to store text info, no formatting
  - HTML is in plain text
- nano
  - most common and user friendly command line text editor, modeless (no switching between insert and command)
  - key strokes:
    - M-6: copy
    - ctrl-o: write
    - ctrl-w: search for text
    - ctrl-u: paste
  - "nano" opens empty file, "nano example.txt" opens file
- tilde and micro are other options--install using "sudo apt install tilde/micro"
- editing .bashrc
  - open the file: "nano ~/.bashrc"
  - add these lines at end of file: "LS_COLORS='rs=0:di=04;31:fi=00;00:ex=01;93';
  export LS_COLORS"
  - remove # from "# force_color_prompt=yes"
  - save and exit, type command: "source ~/.bashrc"
  - LS_COLORS = color values for ls command
  - rs=0 = resets text formatting to normal
  - di=04;31 = directory underlined and red
  - fi=00;00 = sets color of regular files
  - ex=01;93 = sets color of executable files
- traditional unix and linux text editors: ed, vim, emacs
---
# Documenting with Git, GitHub, and Markdown
- GitHub used for documentation
- creating repo
  - click green "new" button
  - name repo in Owner/Repository name field
  - keep public
  - add README file
  - Create repository button
- Markdown syntax
  - "#" = heading
  - "__bold__" = bold
  - "_italic_" = italic
  - hyphen for a list
  - number for numbered list
  - named link: [name of link](link)
  - reference style link: [name of link](name of link), add link elsewhere
  - images: ![Alt text](image-url.jpg)
  - inline code: `inline code`
    - 3 backticks for larger sections
  - block quote: >
  - horizontal line: ___ on a new line
- preview and save: preview, then "Commit changes..." button, leave descriptive commit mesages with more substantive changes
- Commit changes
- add more info to README as you change the repo
- .md rendered by GitHub automatically
- Git Configuration
  - connect to machine, input "git config --global user.name "Your Name"
  git config --global user.email youremail@example.com"
  - use main as default brance and nano as text editor: "git config --global init.defaultBranch main
  git config --global core.editor "nano""
  - verify: "git config --list"
  - generating an SSH key
    - getting the key: "ssh-keygen -t ed25519 -C "your_email@example.com""
    - view key using "cat ~/.ssh/id_ed25519.pub" and copy
    - acccess settings in GitHub, click SSH and GPG keys
    - click New SSH key, add title, select key type "authentication", paste and add SSH key
    - set up SSH key as signing key:
      - git config --global gpg.format ssh
      - git config --global user.signingkey $HOME/.ssh/id_ed25519.pub
      - git config --global commit.gpgsign true
  - clone repo
    - go to repo, click "Code" button, make sure SSH option is selected
    - copy the command, run "git clone git@github.com:repo_user/repo_name.git"
  - stage, commit, and push repo
    - using nano to make changes to repo: cd repo_name
    - open new file: nano filename.md
    - add text
    - push changes: git add entry_one.md
    - commit changes: git commit -m "commit message here"
    - push changes: git push origin main
    - monitor status of repo: git status
  - pulling from repo: cd repo_name
  git pull origin main
- Git Basics
  - repos: local (project directory/folder on computer) and remote (where we send/retrieve/sync files and directories contained in local repo)
  - branches: different parts of the project that can be made to not impact the main branch









