---
title: MS Sql Splash Screen on server 2016
date: 2019-07-29 16:46:13
tags:
 - Server
 - SSMS
categories: 
 - MS SQL
---

# Only splash screen
1. **Replace file** `Microsoft.VisualStudio.Shell.Interop.8.0.dll`
2. **Copy from path** `C:\Program Files (x86)\Microsoft SQL Server Management Studio 18\Common7\IDE\PrivateAssemblies\Interop\`
3. **Replace to path** `C:\Program Files (x86)\Microsoft SQL Server Management Studio 18\Common7\IDE\PublicAssemblies\`

# Reference
[SQL Server Management Studio 18 won't open (only splash screen pops up)](https://dba.stackexchange.com/questions/237086/sql-server-management-studio-18-wont-open-only-splash-screen-pops-up)