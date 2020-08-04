# Dear Azure
Dear Azure, I can't deploy a static application 
if I don't have a file in the root folder. 

My configuration is:
```
app_location: "/"
app_artifact_location: "public"
```

As you can see, [previously to this commit](https://github.com/josedab/personal-website/tree/531e6fa79af73e5a7c87e9df9ea7cd6df10f0e3a) [I was not able to deploy a static site](https://github.com/josedab/personal-website/runs/943479305?check_suite_focus=true).

The error message is related to:
```
Failed to find a default file in the app artifacts folder (/). Valid default files: index.html,Index.html.
```

But I believe what you want to check is:
```
Failed to find a default file in the app artifacts folder (/public). Valid default files: index.html,Index.html.
```

This file exists so I can deploy.
