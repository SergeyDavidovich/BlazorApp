<h3> <b>Blazor Webassembly publish to GitHub Pages sample</b> </h3>

Using:

https://www.meziantou.net/publishing-a-blazor-webassembly-application-to-github-pages.htm

Demo: 

https://sergeydavidovich.github.io/BlazorApp/
` 
name: Deploy to GitHub Pages
on:
  push:
    branches: [ master ]

jobs:
  deploy-to-github-pages:
    # use ubuntu-latest image to run steps on

    runs-on: ubuntu-latest
    # runs-on: windows-latest
    steps:
    # uses GitHub's checkout action to checkout code form the master branch
    - uses: actions/checkout@v2
    
    # sets up .NET Core SDK 6
    - name: Setup .NET Core SDK
      uses: actions/setup-dotnet@v1
      with:
        dotnet-version: 6.0.x  

    # publishes Blazor project to the release-folder
    - name: Publish .NET Core Project
      run: dotnet publish BlazorApp/BlazorApp.csproj -c Release -o release --nologo
     # run: dotnet publish src/Meziantou.Moneiz/Meziantou.Moneiz.csproj --configuration Release
    
    # changes the base-tag in index.html from '/' to 'BlazorApp' to match GitHub Pages repository subdirectory
    - name: Change base-tag in index.html from / to BlazorApp
      run: sed -i 's/<base href="\/" \/>/<base href="\/BlazorApp\/" \/>/g' release/wwwroot/index.html
    
    # copy index.html to 404.html to serve the same file when a file is not found
    - name: copy index.html to 404.html
      run: cp release/wwwroot/index.html release/wwwroot/404.html

    # add .nojekyll file to tell GitHub pages to not treat this as a Jekyll project. (Allow files and folders starting with an underscore)
    - name: Add .nojekyll file
      run: touch release/wwwroot/.nojekyll
      
    - name: Commit wwwroot to GitHub Pages
      uses: JamesIves/github-pages-deploy-action@3.7.1
      with:
        GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
        BRANCH: gh-pages
        FOLDER: release/wwwroot
 `
  
  
  https://github.com/tchapi/markdown-cheatsheet
