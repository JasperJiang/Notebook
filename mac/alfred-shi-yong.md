# Alfred 实用

如何使用Alfred排除某个特定得程序？

```text
1. 打开spotlight，设置-隐私，加入虚拟机文件夹。
2. 打开alfred，输入reload，ok
```

Iterm2 script

```swift
on alfred_script(q)
  set test to (system attribute "server")

  set command to q
  tell application "System Events"
    -- some versions might identify as "iTerm2" instead of "iTerm"
    set isRunning to (exists (processes where name is "iTerm")) or (exists (processes where name is "iTerm2"))
  end tell
  
  tell application "iTerm"
    activate
    set hasNoWindows to ((count of windows) is 0)
    if isRunning and hasNoWindows then
      create window with default profile
    end if
    select first window
    
    tell the first window
      if isRunning and hasNoWindows is false then
        create tab with default profile
      end if
      tell current session to write text q
    end tell
  end tell

end alfred_scripts
```

