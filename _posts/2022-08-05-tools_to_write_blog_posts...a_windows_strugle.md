---
layout: post
title: "tools to write blog posts...a windows struggle"
comments: true
description: ""
keywords: ""
---

I've been trying to figure out the reasons I can't get myself to write more. One of the big ones is finding a blogging platform with a good windows application that can work with my github pages... it doesn't exist. 

I have searched low and high, looked for half baked solutions on github but nothing seems to do what I want. 

Here are my requirements

* Windows desktop app
* Write posts in Markdown
* Easy insertion of photos
* A few clicks from launch to post



I ended up coming up with my own solution. Its essentially just a batch file paired with Typora. This could really be reworked to fit any workflow or chosen markdown editor. I just like typora and typora makes it easy to insert and upload images through the use of [upgit](https://github.com/pluveto/upgit). 


The flow is:

1. Start batch file

2. prompts for a blog post title

3. Creates a file with the current date and name

4. Opens typora with blog post header and title

5. Do some typing and blathering

6. Insert images (drag and drop)...upgit/typora uploads to github and replaces URL with the image URL on github

7. go back to open terminal window hit enter

8. Enter commits changes to git and pushes up to github

9. A nice blog post

   

   Here is the batch file I created. 

   ```cmd
   @echo off
   
   REM Get and set the root post directory
   set root= PATH_TO_YOUR_POST_DIRECTORY
   cd %root%
   
   REM get the current OS date and time
   for /f "tokens=2 delims==" %%a in ('wmic OS Get localdatetime /value') do set "dt=%%a"
   set "YY=%dt:~2,2%" & set "YYYY=%dt:~0,4%" & set "MM=%dt:~4,2%" & set "DD=%dt:~6,2%"
   set "HH=%dt:~8,2%" & set "Min=%dt:~10,2%" & set "Sec=%dt:~12,2%"
   
   REM get the title of the post and strip out spaces
   set /p UserInputTitle=Post title: 
   set UserInputTitleNoSpaces=%UserInputTitle: =_%
   
   REM set the date as a formatted string
   set "datestamp=%YYYY%%MM%%DD%" & set "timestamp=%HH%%Min%%Sec%"
   set "fullstamp=%YYYY%-%MM%-%DD%-"
   
   REM combine the date and post name and ad markdown extension
   set filestamp=%fullstamp%%UserInputTitleNoSpaces%
   set SAVESTAMP=%filestamp%.md
   
   echo --->> %SAVESTAMP%
   echo layout: post>> %SAVESTAMP%
   echo title: "%UserInputTitle%">> %SAVESTAMP%
   echo comments: true>> %SAVESTAMP%
   echo description: "">> %SAVESTAMP%
   echo keywords: "">> %SAVESTAMP%
   echo --->> %SAVESTAMP%
   
   REM start typora with the stamp
   start typora.exe %SAVESTAMP%
   
   
   
   echo hit enter to commit new post
   pause
   
   cd PATH_TO_ROOT_GIT_DIRECTORY_OF_PROJECT
   git pull
   git add .
   git commit -m "new post"
   git push 
   
   ```

   
