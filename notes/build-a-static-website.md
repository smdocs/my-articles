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
