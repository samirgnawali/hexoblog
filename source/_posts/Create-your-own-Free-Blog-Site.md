---
disqusIdentifier: create-your-own-free-blog-site
title: Create your own Free Blog Site
date: 2017-10-30 16:34:29
tags: Hexo blog
keywords:
- hexo blog
- create your own free blog site
- create free blog
sitemap: false
---
There are a lot of blogging platfroms like [wordpress](https://wordpress.com/) and [blogger](https://www.blogger.com). Personally, I was never satisfied with the flexibility of these platforms. So I always wanted a free option to create and maintain blogs. 

We can create and maintain our own blog by using site [Netlify](https://www.netlify.com/). It is basically a frontend hosting site, with a lot of cool features. It lets us create simple websites free of cost and it provides many advanced features for paid users. To create a blog like the one you are visiting right now for free, we will be using [Hexo](https://hexo.io/) which is a fast, simple & powerful blog framework.

<!-- more -->
## Lets Start our blog

### Fork hexo blog

Visit this [link](https://github.com/vksbhandary/hexoblog) and fork my repository or follow following steps to create a new hexo blog.

#### Setup Hexo

Execute following commands in bash to setup hexo and create new blog

``` bash
$ npm install hexo-cli -g
$ hexo init blog
$ cd blog
$ npm install
```
For more information visit [link](https://github.com/hexojs/hexo#installation), it has all the necessary steps to get you started.

#### Install theme

For my convenience i have installed theme [hexo-theme-tranquilpeak](https://github.com/LouisBarranqueiro/hexo-theme-tranquilpeak). It is well documented and support various features. You can choose any theme you want from [site](https://hexo.io/themes/). Execute following commands to install theme.

``` bash
$ cd themes
$ git clone https://github.com/LouisBarranqueiro/hexo-theme-tranquilpeak
$ cd hexo-theme-tranquilpeak
$ npm install
$ bower install
$ cd ../..
$ npm install hexo-algoliasearch
```
### Setup blog and theme

There are number of things you must modify like site URL, Author name, Site title etc. The themes also provide many options to give users flexibility to change things on their blogs. Themes can support various services like analytics, facebook insights etc. Generally settings are to be set in _config.yml file. 
In order to get more info on integrated services configuration in hexo-theme-tranquilpeak theme visit this [link](https://github.com/LouisBarranqueiro/hexo-theme-tranquilpeak/blob/master/docs/user.md#integrated-services-configuration).

### Add blog to repository

For netlify we need to add our UI on repository, so pick any git client and upload your blog.

For information on creating repository on github fillow [link](https://help.github.com/articles/create-a-repo/).

### Add site on Netlify

Signup and create site using your github. Their wizerd shows three steps, 1 Connect to your git provider, 2 Pick your repository, 3 Build options, and deploy!

we need to provide Build options

``` bash
 npm install; cd themes/tranquilpeak; npm install; bower install; grunt buildProd ; cd .. ; cd .. ; hexo generate; hexo algolia;

```
And finally set publish directory as "/public"

Note: In my repo i have renamed "hexo-theme-tranquilpeak" theme to "tranquilpeak" so please use your head while copying.

### Domain Registration

In nepal, there is a company called [Mercantile](http://www.mos.com.np/) which provides .com.np domains free of cost to nepali citizens. So you can visit this [link](http://register.mos.com.np/) and fill out the registration form, and wait for two days atleast and you will get email confirmation about the domain registration. You can not choose domain of your own you must go for yournamesurname.com.np only. Other citizens can buy domain from any provider.

Registration process asks for nameserver, in order to access nameserver Goto Domain management->Custom Domain->Add a custom domain. Set your domain, and click "Use Netlify DNS". After that, you will have three nameserver values, use these to set your nameserver in domain provider's setting.

### Add DNS records

You might want to use www version of your domain or vice-versa  for that you can add DNS record on Netlify so you can enable HTTPS on your site.

So you can add any one of following two records.

``` bash
Type: CNAME
Host: www.yourdomain.com.np
Value: yourdomain.com.np
TTL: 3600
```

or 

``` bash
Type: CNAME
Host: yourdomain.com.np
Value: www.yourdomain.com.np
TTL: 3600
```
### Enable HTTPS

Goto Domain management->HTTPS and click on "Use lets encrypt" or "Set custom certificate". After a minute or so HTTPS will be enabled.

### Force HTTPS

Goto Domain management->Force HTTPS and click on "Force HTTPS" button.

## Happy blogging

Now you can use your blog. If you are a developper you can modify your blog for your own good.
