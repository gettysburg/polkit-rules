# polkit-rules
This repository contains polkit rules that I either actively use on my system and / or made for other people and projects.

**50-hidepid.rules**

polkitd rule in order to make hidepid effective again on systemd-based distributions.

If you did not know, systemd allows you to essentially bypass the restrictions imposed by hidepid - systemd developers currently don't consider this a regression, security issue or information leak [1].

On a multi-user system, you can for example, view other users processes despite hidepid=2 being active:

````
[user@Workstation ~]$ systemctl status user-1000.slice

● user-1000.slice - User Slice of UID 1000
     Loaded: loaded
    Drop-In: /usr/lib/systemd/system/user-.slice.d
             └─10-defaults.conf
     Active: active since Wed 2025-04-02 19:20:31 CEST; 19h ago
 Invocation: 58d393519763427680e0316781c09a58
       Docs: man:user@.service(5)
      Tasks: 893 (limit: 41790)
     Memory: 6.9G (peak: 7.7G)
        CPU: 1h 31min 27.188s
     CGroup: /user.slice/user-1000.slice
             ├─session-1.scope
             │ ├─ 1104 "login -- user"
             │ ├─ 1156 xinit /etc/xdg/xfce4/xinitrc -- /etc/X11/xinit/xserverrc vt1
             │ ├─ 1170 /usr/lib/Xorg -nolisten tcp :0 vt1
             │ ├─ 1183 xfce4-session
             │ ├─ 1226 /usr/bin/ssh-agent -s
             │ ├─ 1236 xfwm4
             │ ├─ 1251 xfsettingsd
             │ ├─ 1264 xfce4-panel
             │ ├─ 1275 xfdesktop
````

The goal of this rule is to make hidepid **effective** again, whether systemd developers like the concept of group-based `/proc` access control, or not.

You will have to replace `adminuser` and `trusted` with a user and group name of your desire.

[1] - https://github.com/systemd/systemd/issues/29893
