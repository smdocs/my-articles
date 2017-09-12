Everything starts with the idea to build a website to host my work. On the road, it became obvious that the first article has to be precisely on how to do it. And here it is.
To be effective I picked [Hugo]() that I already know. It is as well, very fast, cross-platform, open source and ranked in the top 3 of static website generators: a very solid pick.

Everything as to stay free, so the generated pages are hosted in a Github repository side by side with the website source code. It comes 
with a Github sub-domain served over a secured HTTPS connection for free as well.

For my own needs I have added two extra goals at the end of the article. They are not interesting for everyone, feel free to drop them:
- A beautiful code syntax highlighting of a wide range of languages. (free)
- A secured custom domain name (few bucks per year)

### what we get at the end
- No costs.
- A complete control.
- A very fast static site.
- A secured website.
- A website compliant to best practices.
- A minimalist design.
- A beautiful code syntax highlighting of a wide range of languages

#### 1. Set up development environment
  - Create the following directory structure

```
mkdir -p ~/Sources/fillmem
cd ~/Sources/fillmem
mkdir bin
mkdir site
```
#### 2. Install HUGO
Check what’s the last Hugo release and download the right version for your environment. My environment is a 64 bits MacOS.
```
curl -L -s -o bin/hugo.tar.gz https://github.com/gohugoio/hugo/releases/download/v0.26/hugo_0.26_macOS-64bit.tar.gz
tar xvzf bin/hugo.tar.gz -C bin/
```

Verify that the Hugo generator is working properly by making it print its version
```
./hugo version
Hugo Static Site Generator v0.26 darwin/amd64 BuildDate: 2017-08-07T08:
```

#### 3. Create the Github repository

I chose to do a user/organization page as I’m building my website. If you stick to this choice you can follow the next steps. 
Otherwise you’re on your own!

To get user page features activated the repository has to be named in a specific way. It’s easier to understand with the example of this site.
My account username is gsempe at the address https://github.com/gsempe.

This site is hosted in the repository named gsempe.github.io at the address https://github.com/gsempe/gsempe.github.io

You have to follow this scheme.

If your Github username is jcarmack, creates the repository jcarmack.github.io. Make it public, do not add a README file, neither 
a .gitignore or a license file. The master branch is the branch where generated pages are hosted, you don’t want any non 
mandatory files there.

#### 4. Initiate repository branches

Push the first commit on the master branch to create it. Generated pages will be pushed on this branch.
```
cd site
git init
git remote add origin git@github.com:jcarmack/jcarmack.github.io.git
git commit --allow-empty -m "Start the public branch"
git push -u origin master
```

Create the source branch. Website source code will be pushed on this branch.
The source branch has to have a .gitignore file to avoid to commit generated or temporary files. I use the online tool gitignore.io to generate .gitignore files. It is very convenient to get the usual files to ignore regarding OSX, vim and Hugo in one curl command.

```
git checkout --orphan source
curl -L -s -o .gitignore https://www.gitignore.io/api/osx%2Cvim%2Chugo
git add .gitignore
git commit -m "Start to fill the memory"
git push origin source
```

Verify that the .gitignore file generated at the previous step contains the line /public/. If the line does not exist add it and commit this change.
```
cat .gitignore | grep public
/public/
```
Use the git worktree feature to checkout the master branch to the ignored public sub-folder.
Why? By default, Hugo generates the site in the public folder. To have the master branch mapped to the public folder makes possible to easily chain site generation and publication steps without moving around files.
```
rm -rf public
git worktree add -B master public origin/master
```

#### 4. Configure and generate the site

The environment setup is done and it’s finally the moment to make the actual site.

Create a new empty site tree
```
git checkout source
../bin/hugo new site . --force
git add config.toml archetypes/default.md
git commit -m "Add fresh empty site"
git push origin source
```
Add the Hugo theme. This site uses minimo theme
Why minimo theme? It’s clean/minimalist. It supports categories/tags, renders correctly on desktop and mobile phones, and has built-in hooks to add custom Javascript and CSS.
```
git submodule add https://github.com/MunifTanjim/minimo themes/minimo
git submodule init
git submodule update
cp themes/minimo/exampleSite/config.toml .
```
Modify the config.toml configuration file to fit your site.
```
baseURL = "https://jcarmack.github.io"
title = "J. Carmack"
[params]
  description = "Carmack static site"
[author.social]
  codepen = ""
  email = ""
  facebook = ""
  github = ""
  instagram = ""
  twitter = "ID_AA_Carmack"
  linkedin = ""

[[menu.main]]
  name = "Blog"
  weight = -70
  identifier = "jcarmack"
  url = "https://jcarmack.github.io"
Commit the changes

git add config.toml .gitmodules themes/minimo 
git commit -m "Add minimo theme"
git push origin source
Test the site

../bin/hugo server -D
```

If you open your web browser and you visit the address http://localhost:1313/ the site should load even if there is no content for the moment.
