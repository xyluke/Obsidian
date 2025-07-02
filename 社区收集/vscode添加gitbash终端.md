
terminal.integrated.shell.windows  


设置-设置-shell windows
![[../img/Pasted image 20250702100213.png]]
~~~json
	"terminal.integrated.profiles.windows": {
    
        "bash": {
            "path":"D:\\Git\\Git\\bin\\bash.exe",
            "args": []
		},
		"PowerShell": {
			"source": "PowerShell",
			"icon": "terminal-powershell"
		},
		"Command Prompt": {
			"path": [
				"${env:windir}\\Sysnative\\cmd.exe",
				"${env:windir}\\System32\\cmd.exe"
			],
			"args": ["/k D:\\02_soft\\0200-3-31-cmdr\\cmder\\vendor\\init.bat"],
			"icon": "terminal-cmd"
		}
    },
~~~

https://blog.csdn.net/qq_36303853/article/details/104067540?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522908e3fbe9185e79179261921b5a00890%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=908e3fbe9185e79179261921b5a00890&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduend~default-1-104067540-null-null.142^v102^pc_search_result_base4&utm_term=vscode%E6%B7%BB%E5%8A%A0%E7%BB%88%E7%AB%AF&spm=1018.2226.3001.4187