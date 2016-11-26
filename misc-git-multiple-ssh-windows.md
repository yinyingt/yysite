---
layout: post
title: Multiple SSH Key Access for Git in Windows
mathjax: false
related: false
comments: false
published: true
---


_Created: Dec 2015_

_Last updated: Dec 2015_


It used to be a pain to access different git repositories with different SSH keys in Windows. There may be other more elegant approaches, but here is a solution working well enough for me. The following example assumes there are multiple SSH keys in "~/.ssh/".

To generate a new SSH key pair:

```
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```


* Install Git in Windows from [here](https://git-scm.com/download/), which will also install bash and SSH utilities. 

* Configure ssh-agent in bash as in this [Github tutorial](https://help.github.com/articles/working-with-ssh-key-passphrases/#auto-launching-ssh-agent-on-msysgit). This will initialize ssh-agent upon the creation of a BASH session. 

* (Optional) Change the user name and email via "git config" commands accordingly. 

* Create simple bash helper functions to switch between SSH keys, and include them in bashrc. 

```
# SSH tweaks
export SSH_KEY_DEFAULT="/c/Users/user/.ssh/id_rsa"  # Need to specific the full path
export SSH_KEY_GITHUB="/c/Users/user/.ssh/github"

# Launch ssh-agent
source /c/Users/user/.ssh_agent_windows_config   # the config file from github

# Functions below assume the ssh-agent is already running
function use_ssh_key_default()
{
  ssh-add -D
  ssh-add $SSH_KEY_DEFAULT
}

function use_ssh_key_github()
{
  ssh-add -D
  ssh-add $SSH_KEY_GITHUB
}
```

The file "/c/Users/user/.ssh_agent_windows_config" is copied from the [Github tutorial](https://help.github.com/articles/working-with-ssh-key-passphrases/#auto-launching-ssh-agent-on-msysgit), or pasted below: 

```
# Note: ~/.ssh/environment should not be used, as it
#       already has a different purpose in SSH.

env=~/.ssh/agent.env

# Note: Don't bother checking SSH_AGENT_PID. It's not used
#       by SSH itself, and it might even be incorrect
#       (for example, when using agent-forwarding over SSH).

agent_is_running() {
    if [ "$SSH_AUTH_SOCK" ]; then
        # ssh-add returns:
        #   0 = agent running, has keys
        #   1 = agent running, no keys
        #   2 = agent not running
        ssh-add -l >/dev/null 2>&1 || [ $? -eq 1 ]
    else
        false
    fi
}

agent_has_keys() {
    ssh-add -l >/dev/null 2>&1
}

agent_load_env() {
    . "$env" >/dev/null
}

agent_start() {
    (umask 077; ssh-agent >"$env")
    . "$env" >/dev/null
}

if ! agent_is_running; then
    agent_load_env
fi

# if your keys are not stored in ~/.ssh/id_rsa or ~/.ssh/id_dsa, you'll need
# to paste the proper path after ssh-add
if ! agent_is_running; then
    agent_start
    ssh-add
elif ! agent_has_keys; then
    ssh-add
fi

unset env
```

Now, one can switch among different SSH keys so that only the correct SSH key resides in the ssh-agent. 

* Below is a manual solution, good for debugging purpose. Manually add the target SSH keys by 

```
ssh-add ~/.ssh/target_private_key
```

After that, check the added SSH keys by 

```
ssh-add -l
```

If the first key is the one that is added by default and is not the right key for this Git repository, remove it by 

```
ssh-add -d ~/.ssh/wrong_private_key
```

and then add the correct SSH key.
