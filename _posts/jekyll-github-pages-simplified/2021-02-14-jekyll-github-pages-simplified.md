---
title: Personal website/blog with Jekyll + Github. 
date: 2021-02-14 08:58:14 +05:30
tags: [jekyll, github, website]
description: Tips and tricks for setting up a free dynamic website/blog with Github Pages using Jekyll.
image: "/airflow-concurrency-simplified/airflow.jpeg"
comment: true

---
I recently moved my website/blog from wordpress to Jekyll + Github Pages (I am still migrating). I love simplicity of both Jekyll and Gihub, that is the precise reson for the move. Wordpress is still awesome and best CMS, but for a simple website like mine, Jekyll and Github Pages are most efficient and cost effective. Making beautiful static website couldn't be any cheaper and easier.

<figure>
<img src="github.jpg" alt="Github Website"> 
</figure>

There are loads of tutorials available online on this topic, so I am not going to get into detailed steps. However we will going  discuss kye concepts, options and tips that will help you in your decisions. I have also provided references to some useful articles on this topic in the end. 

## Concepts

##### Types of Github Pages
There are three types of [GitHub Pages](https://pages.github.com/){:target="_blank"} sites: project, user, and organization. Project sites are connected to a specific project hosted on GitHub, User and organization sites are connected to a specific GitHub account. To publish a user site, you must create a repository owned by your user account that's named `<username>.github.io`. To publish an organization site, you must create a repository owned by an organization that's named `<organization>.github.io`. 


##### ✨ Special ✨ Repositories 
Repository that maches your username is a special repository in Github. `README.md` in root of default branch of this repository is shown on your gihub profile. If you configure Github pages for this repository, your website will be published at `http(s)://<username>.github.io`.

Similarly repository that maches your orgname is a special repository. `README.md` is shown on Oranization's gihub profile and github pages website will be published at `http(s)://<organame>.github.io`.

Project sites are available at `http(s)://<username>.github.io/<repository>` or `http(s)://<organization>.github.io/<repository>`.

> If you are using a custom domain, you can pubish any repository to desired domain, you do not need to worry about the repository name.

##### Jekyll Website Directory Structure
Before you start, please make sure you are familier with [Jekyll direcotry structure](https://jekyllrb.com/docs/structure/){:target="_blank"} and [Markdown syntax](https://www.markdownguide.org/tools/jekyll/){:target="_blank"}.  
 

##### Public vs Private Repo
If you would like to publish pages from a private repo, you can not do it in a free plan. You need a pro account to publish pages from private repository.

## Create Simple website using Gihub themes
You can publish a simple jekyll website in less than an hour just by using Github UI. You won't even need to install Jekyll or Ruby. Here are steps to create a website and add posts on website with a Github provided theme. 

- Click on settings of your repository.
- Scroll down to Github Pages section of settings.
- Click on `Select a Theme` button.
- Select a Theme
- Write content in `index.md` file, when prompted.

Your website with gihub provided theme is now ready. The webiste is published from `gh-pages` branch of your repository. If you look at `gh-pages` branch, you will find two files `_config.yaml` and `index.md`.

To add a new post create a folder `_posts` and  create a new file called YYYY-MM-DD-NAME-OF-POST.md, replacing YYYY-MM-DD with the date of your post and NAME-OF-POST with the name of your post. Your post is now availabel on your website, the new post will be located at `https://<username>.github.io/YYYY/MM/DD/NAME-OF-POST.html`.

If you want to add a new page, create a new file for your page called <PAGE-NAME>.md. You need to add special header formatter to specify the url of the page. e.g If you want to create a page `https://<username>.github.io/about/contact-me/` create a page named contact-me.md and add following header config inside the file.
```
	layout: page
	title: "Contact me"
	permalink: /about/contact-me/
```

## Using Custom Theme
Building website using custom theme is more involved and takes some time, but is also a lot more rewarding. Github provided themes are very limited and you are not able to customize a selected theme currently. 

##### Step # 1: Find a Theme 
First and the most important part of building your website is finding a suitable theme and  it is also most time consuming task. You can choose to buy a theme or find a free theme, in anycase you will need to spend time finding right theme for you. If you are looking for free themes search inside Github rather than Google. 

##### Step # 2: Install Jekyll Locally
This might sound simple but a lot of people face problems, specially on Mac OS X. You will need to install `ruby`, `rvm` and `jekyll`,  [Installation instructions](https://jekyllrb.com/docs/installation/){:target="_blank"} on Jekyll website are accurate and concise. Make sure your versions are compatible with [Github dependency versions](https://pages.github.com/versions/){:target="_blank"}. 


##### Step # 3: Fork the theme Repo
This step is only needed if you selected a free theme from Github, which is very likely. If you fall into this category, please create a fork of repo and rename the repository to your `username` (or `orgname` or `project`). 

##### Step # 4: Branches for your repo: 
If this is your  `username` or `orgname` repo, I recommend using following branches:
- **main**: Make it default, and this is where I keep README.md for your Github profile.
- **master**: This is where you keep the forked theme from oroginal repo.
- **website**: This is where you add/updated your contents e.g pages, posts (`_posts`), images and configuration (`_config.yaml`). Note that this branch has entire jekyll directory structure but I make changes to only above mentioned items.

This structure helps me keep the theme in sync with the orignal repo and also make contributions. If you like to update your website to new version of theme, all you have to do is refresh your master and rebase website branch against master. 

##### Step # 5: Add New Posts / Customize
Clone tis new repo to you pc/laptop using your repository url e.g. `git clone https://github.com/{username}/{username}.git`.

Add new posts, pages, modify configuration, add disqus and what not. Test your changes locally and when ready push the changes to github.

##### Step # 6: Configure Github Pagess in Settings:
Click on settings of your repository, Scroll Down to Github Pages section of settings and select `website` from the branches dropdown. 

##### Step # 5a and 6a: My Theme is not compatible with Github CI.
Many great themes are not supported by Github CI due to the dependencies theme use. Don't discard these themes, there is a very simple and easy work around. 
- After you run `jekyll build` or `bundle exec jekyll build`, your static website is generated in a folder named `_site`. Rename this folder to `docs` and check it in (Yes! check in generated files).
- In the Github Pages sections of your Github Settings select docs from root folder drop down. You are no longer using using the Github CI, but this shouldn't matter  as your website is exectly how you wanted it. 

Please remember your final website is only HTML, CSS and Javascript. Every thing else you have in your repo is to help you create that static website. So you can take content in `_site` and host any anywhere you like, Github tends to be most convenient and familiar place. 

## Connect your domain and enable SSL
In Github Pages section of your repository settings you can choose to publish your pages to your domain. If the domain name is `my_domain.com`, I prefer to use `www.my_domain.com` in this section. Github pages do not yet support subdomain linking. I think that is unlikely to change anytime soon. 

You will need to update the DNS on your domain provider e.g. goddaddy. 

Github also provides free SSL, I recommend enabling SSL on your website. It takes couple of hours to generate the certficate after you have added a domain name. 

## References
- [Github docs: Creating Github Site using Jekyll](https://docs.github.com/en/github/working-with-github-pages/creating-a-github-pages-site-with-jekyll){:target="_blank"}
- [Github docs: Add Content to Github Site using Jekyll](https://docs.github.com/en/github/working-with-github-pages/adding-content-to-your-github-pages-site-using-jekyll){:target="_blank"}
- [Jekyll Markdown Guide](https://www.markdownguide.org/tools/jekyll/){:target="_blank"}
- [Jekyll Directory Structure](https://jekyllrb.com/docs/structure/){:target="_blank"}
- [A Neat and Concise blog](https://ndench.github.io/jekyll/setup-jekyll-on-github-pages){:target="_blank"}
- [A detailed Blog, TLDR;](https://www.aleksandrhovhannisyan.com/blog/getting-started-with-jekyll-and-github-pages/){:target="_blank"}
 














