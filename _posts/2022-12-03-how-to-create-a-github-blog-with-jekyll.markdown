---
title: "How to create a github blog with Jekyll"
tags:
  - minimal mistakes
  - jekyll
  - blog
  - github
  - bundler
---

💡 This posts introduces how to create github pages, host it on web and apply Jeykll theme to the website. 
{: .notice--warning}

### 1. Sign in Github: [https://github.com](https://github.com)
<br>
### 2. Create a new repository: [https://github.com/new](https://github.com/new)
![newrepo](https://user-images.githubusercontent.com/40441643/205811405-68dc2cb6-49b7-4930-bc04-a8ce274a3cfc.PNG)
- Repository name should be owner.github.io, e.g. choycoy.github.io.
- Click the option as public to make everyone view it.
- Good to add a `README file` to write the description for your project.

### 3. Upload the file to test
e.g. upload the `index file` which is written Hello World.
```
1. <!DOCTYPE html>
2. <html>
3. Hello, World!
4. </html>
```
<br>
Then upload `index.html` on your repository:![upload](https://user-images.githubusercontent.com/40441643/205814189-c110c113-7201-4959-bf81-27a9ab0291e6.PNG)

### 4. Check the link of your site
![link](https://user-images.githubusercontent.com/40441643/205815518-e4bd54da-b8ac-4df1-b6a5-979a6a5d3a59.PNG)
:can check the link of your website on pages in Settings.
<br>
<br>
Then you can see the website which is written “Hello, World!” when you visit, e.g. https://usename.github.io.

### 5. Apply Jekyll theme to make your blog look cool
- Note that the test page was created using bundler in order to continue customising the site on your own without increasing the number of commit unnecessarily, which is not a good habit when u use github.
<br>
- Using Jekyll with bundler, the server for developing the site was created and the default skin was ‘minima’. See the attachment: [https://jekyllrb.com/tutorials/using-jekyll-with-bundler/](https://jekyllrb.com/tutorials/using-jekyll-with-bundler/)
<br>
- To apply the Minimal Mistakes Jekyll theme on my website, I have downloaded the Minimal Mistakes source code from [https://github.com/mmistakes/minimal-mistakes/releases](https://github.com/mmistakes/minimal-mistakes/releases) and extracted them all on my test blog folder with replacing the duplicated files. 
<br>
<br>
Then the unnecessary files were removed from the source code folder, which were
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

### 6. Edit the `Gemfile`
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
### 7. Enable the profile on your website by adding `author_profile: true` in the `index file`.
In the `index` file:
```
layout: home
author_profile: true
```
Then I have got the awesome blog which is shown below,
![final](https://user-images.githubusercontent.com/40441643/205818355-3213c27f-dbdc-4d54-85e8-062422a3351c.PNG)


