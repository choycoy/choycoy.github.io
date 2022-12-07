---
title: "How to create a github blog with Jekyll"
tags:
  - minimal mistakes
  - jekyll
  - blog
  - github
  - bundler
---

💡 This post introduces how to create github pages, host it on web and apply Jeykll theme to the website. 
{: .notice--warning}

### 1. Sign in Github: [https://github.com](https://github.com)
<br>
### 2. Create a new repository: [https://github.com/new](https://github.com/new)
![newrepo](https://user-images.githubusercontent.com/40441643/205811405-68dc2cb6-49b7-4930-bc04-a8ce274a3cfc.PNG)
- Repository name should be owner.github.io, e.g. choycoy.github.io.
- Click the option as public to make everyone view it.
- Click the option to add `README file`.

### 3. Create a folder to store the repository on my desktop
First, create a folder on my desktop to clone my repository I just made.
e.g. choycoy.github.io on my desktop
![https](https://user-images.githubusercontent.com/119985540/206070954-7db9cf5e-2782-4c4d-8ad8-60b0fb9144e9.PNG)
:can check the URL of my repository and copy it.

### 4. Clone the repository
Then open Git bash and enter 
```
git clone https://github.com/choycoy/choycoy.github.io.git 
```
on the path of folder just created.

### 5. Create 'index' file on cmd
```
$ echo "Hello, World!" > index.html
```

### 6. Push 'index' file to the remote repository
```
git add *
git commit -m "Start git blog"
git push -u origin main
```
### 7. Check the blog link
![link](https://user-images.githubusercontent.com/40441643/205815518-e4bd54da-b8ac-4df1-b6a5-979a6a5d3a59.PNG)
:can check the link of my blog on pages in Settings.
<br>
<br>
Then I can see the blog which is written “Hello, World!” when I visit it.

### 8. Apply Jekyll theme to make my blog look cool
- Note that the local server was used to continue customising the site on my own without increasing the number of commit unnecessarily, which is not a good habit when we use github.
<br>
- Using Jekyll with bundler, the blog with the default skin was `minima` was created. See the attachment: [https://jekyllrb.com/tutorials/using-jekyll-with-bundler/](https://jekyllrb.com/tutorials/using-jekyll-with-bundler/)
<br>
- To apply the `Minimal Mistakes` Jekyll theme on my blog, I have downloaded the Minimal Mistakes source code from [https://github.com/mmistakes/minimal-mistakes/releases](https://github.com/mmistakes/minimal-mistakes/releases) and extracted them all on my test blog folder with replacing the duplicated files. 
<br>
<br>
Then unzip the source folder to the folder where my repository was stored and removed the unnecessary files from the source code folder, which were
- `.editorconfig`
- `.gitattributes`
- `.github`
- `/docs`
- `/test`
- `CHANGELOG.md`
- `minimal-mistakes-jekyll.gemspec`
- `README.md`
- `screenshot-layouts.png`
- `screenshot.png`

### 9. Edit the `Gemfile`
From
```
1. source "https://rubygems.org"
2. gemspec
```
to
```
1. source "https://rubygems.org"
2. gem "jekyll", "~> 4.3.1"
3. gem "minimal-mistakes-jekyll"
```
### 10. Change `_congig' file to customise the blog
![skin](https://user-images.githubusercontent.com/40441643/206083427-e10ecadc-b5c8-4938-a929-f0cc77be23e6.PNG)
e.g. uncommenting the remote_theme

### 11. Push every files to the remote repository
In the path of the remote repository folder on Git Bash,
```
git add *
git commit -m "Apply jekyll theme"
git push -u origin main
```
Then, I have got the awesome blog as seen below.
<br>
![final](https://user-images.githubusercontent.com/40441643/205818355-3213c27f-dbdc-4d54-85e8-062422a3351c.PNG)

