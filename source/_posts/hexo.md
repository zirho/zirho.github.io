---
title: How to setup a blog on github with Hexo
---

You can just copy & paste the codes below to setup a blog.

Related links  
* [Hexo docs](https://hexo.io/docs/)
* [Hexo themes](https://hexo.io/themes/)
* [github pages](https://pages.github.com/)

## Prerequisites

<div>
<h3 id="Install-Git" class="article-heading"><a href="#Install-Git" class="headerlink" title="Install Git"></a>Install Git<a class="article-anchor" href="#Install-Git" aria-hidden="true"></a></h3>
<ul>
<li>Windows: Download &amp; install <a href="https://git-scm.com/download/win" target="_blank" rel="external">git</a>.</li>
<li>Mac: Install it with <a href="http://mxcl.github.com/homebrew/" target="_blank" rel="external">Homebrew</a>, <a href="http://www.macports.org/" target="_blank" rel="external">MacPorts</a> or <a href="http://sourceforge.net/projects/git-osx-installer/" target="_blank" rel="external">installer</a>.</li>
<li>Linux (Ubuntu, Debian): <code>sudo apt-get install git-core</code></li>
<li>Linux (Fedora, Red Hat, CentOS): <code>sudo yum install git-core</code></li>
</ul>
<h3 id="Install-Node-js" class="article-heading"><a href="#Install-Node-js" class="headerlink" title="Install Node.js"></a>Install Node.js<a class="article-anchor" href="#Install-Node-js" aria-hidden="true"></a></h3><p>The best way to install Node.js is with <a href="https://github.com/creationix/nvm" target="_blank" rel="external">nvm</a>.</p>
<p>cURL:</p>
<figure class="highlight bash"><table><tbody><tr><td class="code"><pre><span class="line">$ curl https://raw.github.com/creationix/nvm/master/install.sh | sh</span><br></pre></td></tr></tbody></table></figure>
<p>Wget:</p>
<figure class="highlight bash"><table><tbody><tr><td class="code"><pre><span class="line">$ wget -qO- https://raw.github.com/creationix/nvm/master/install.sh | sh</span><br></pre></td></tr></tbody></table></figure>
<p>Once nvm is installed, restart the terminal and run the following command to install Node.js.</p>
<figure class="highlight bash"><table><tbody><tr><td class="code"><pre><span class="line">$ nvm install 4</span><br></pre></td></tr></tbody></table></figure>
<p>Alternatively, download and run <a href="http://nodejs.org/" target="_blank" rel="external">the installer</a>.</p>
<h3 id="Install-Hexo" class="article-heading"><a href="#Install-Hexo" class="headerlink" title="Install Hexo"></a>Install Hexo<a class="article-anchor" href="#Install-Hexo" aria-hidden="true"></a></h3><p>Once all the requirements are installed, you can install Hexo with npm.</p>
<figure class="highlight bash"><table><tbody><tr><td class="code"><pre><span class="line">$ npm install -g hexo-cli</span><br></pre></td></tr></tbody></table></figure>
</div>


#### Setting up a github repository

You should change **{blogname}** with your desired word.

Setup a github repo with the name, **{blogname}**.github.io  
ex) zirho.github.io     
[https://github.com/zirho/zirho.github.io](https://github.com/zirho/zirho.github.io)     

## Setting up Hexo with Github  

#### Setup a blog 

```
$ hexo init {blogname}
$ cd {blogname}
$ npm i
$ git init
```
This will generate the {blogname} folder and install dependencies.

#### Install a theme 

Browse [here](https://hexo.io/themes/) to find out something cool.

Once you decide your mind, fork it to customize it or just get the github repo url from the theme info.

ex) [https://github.com/ppoffice/hexo-theme-minos](https://github.com/ppoffice/hexo-theme-minos)
```bash
$ git submodule add {theme-github-url} themes/{theme-name}
```
  
   
Copy _config.yml.example to _config.yml
```bash
$ cp themes/{theme-name}/_config.yml.example themes/{theme-name}/_config.yml
```
***Some themes may differ on _config.yml.example file name**  
**Refer to the theme docs**  


#### Setup blog & deploy info 

Edit _config.yml in root folder. (Don't get confused with theme config file)

```bash
$ vi _config.yml
```

Update blog info as desired.  
Below is my own for instance.  

```bash
title: CodePaste 
subtitle:
description:
author: Joshua Youngjae Ji
language: en
timezone: America/Los_Angeles
```

Add below code at the bottom of the file for deploy on github repo.  

```yml
deploy:
  type: git
  repo: {your github repo url}
  branch: master
  message: "Site updated: {{ now('YYYY-MM-DD HH:mm:ss') }}"
```

#### Deploy the blog

```bash
$ npm i -S hexo-deployer-git
$ hexo deploy
```

At this point, you should be able to see your blog at http://{blogname}.github.io. 

#### Add the source to the github repository (optional)

To maintain version of the code, you can make another branch and push the commits.  

```bash
$ git remote add origin {your-git-repo-url}
$ git checkout -b source 
$ git push origin source 
```

#### Deploy new post 

Adding a new post

```bash
$ hexo new {postname}
```

Edit the new post file

```bash
$ vi source/_posts/{postname}.md
```

Regenerate files and deploy at once

```bash
$ hexo generate -d
```


Happy posting!


