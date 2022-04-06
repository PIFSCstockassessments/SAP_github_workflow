# Github Workflow

## Setting up a repository  
To initiate a repository, go to the [PIFSCstockassessments page](https://github.com/PIFSCstockassessments) and click `New`. Set it up using the pifsc-repo-template, which will automatically populate a README.md file with the NOAA disclaimer, a LICENSE.md file, and a .gitignore template for R. 

## Software to use with GitHub
Git is a version control system where you can manage and keep track of code history and GitHub is a service to manage Git repositories. There are many ways to interface with GitHub, you can go straight from command line or use a third-party software to help push and pull changes. Some good options are:
- Git for Windows (base Git tool for Windows users)
- GitHub desktop 
- Visual Studio Code (this is free, open source code editor that has git commands built in)
- RStudio (you can set up git integration in RStudio and push and pull straight from the IDE)
- GitKraken (software meant specifically for Git)

## Suggested workflow
Once you have a repo set up and are ready to start making changes, you can make your first commit. First you will need to clone the repository onto your computer. There are multiple ways to achieve this. If you are using command line you will navigate to where you want to keep the repository on your computer and use the git clone command. It should look something like this: 
``` 
cd C:/Users/user/Documents/my_git_repo

git clone <url for repo>
```
The url for your repo can be found by clicking the green Code button on the main page of the repo and it is under the Clone section. 
![gitclone](docs/img/clone.PNG)

If you are using one of the software mentioned above, there will be an option to "Clone a Repository" and you will simply need to paste the url into the box provided. 
![clonerepovs](docs/img/clone_repo.PNG)

Once you have the repo cloned onto your computer, you can start making changes and adding files. To make a change, try opening the README.md file and add a brief summary of the purpose of the repository. Save those changes and in the program you are using, it should notify you there is a change to be commited usually by a number over the Git icon. 
![commitvs](docs/img/vs_change.PNG)

Pushing to a repository involves multiple steps. 1) You will stage the change by clicking the `+` next to the file that was changed. 2) Once it is staged, you will need to add a commit message and commit the change. 3) You can push the change. *Note: it is good practice to pull changes from a repo at the beginning of each work session to ensure you have the most updated changes.* Also, committing often can prevent issues if you need to revert back to a previous version and can make it more transparent what changes you made.

### Troubleshooting Commits 
If you have an issue, try pulling changes first then pushing your changes. 
If you still cannot push your changes, make sure your file sizes are not too large. A lot of stock assessment model files can get pretty large so if they are not crucial to someone running the code, do not include them in your commits. 
If you are committing multiple files, undo the commit and do them individually.

## Branches 
Branches allow you to work on code development without affecting code in other branches (including the Main branch). Once you are ready to, you can then merge the development branch into another one using a pull request. A good way to use the branch feature in this workflow is to create a branch for each step of the assessment (e.g. CPUE standardization, Catch data, Model Development, Model Evaluation, etc.) and once that section is finished you can merge everything into the main branch. This will help keep things separated and more organized. Also, submitting a pull request to the main branch will give all team members a chance to see the changes that have been implemented at each stage. 
![branch](docs/img/git_branch.PNG)

## Hosting supporting documents on repo website  
GitHub will create a static website for repositories which will allow you to publish html files. This is useful because if you use RMarkdown files to write up summaries or reports, looking at the .Rmd or .Rmarkdown file directly from GitHub does not provide the full output of the rendered file and you cannot directly view the html file produced. Instead, you can publish the html file on your repo website and then anyone can view the file as it was meant to be read without having to download any files. 
To host documents on the repo website, you need to first turn on the GitHub Pages site. To do this, go to `Settings` -> `Pages`. Choose the branch and directory for the site to be built from and you can select which theme to use. Once your site is published it will provide the web address of where it is published.
![pages](docs/img/githubpages.PNG)
 Next, create a file, `index.md` and add links to the documents you would like to display. For example, if you have data exploration summaries in an RMarkdown format that has been knit to an html file, you can add those. Include any text you would like in Markdown syntax and give the link to the file you want published on the website. The first part of the link will be the web address provided by GitHub when it set up the pages (in the figure above it is "https://pifscstockassessments.github.io/MHI_Bottomfish_2023/"). The second part of the link will be the path to the file you want to host relative to the directory from where the website is being published. In the example above, it is published to the main branch and root directory. So the path to the file I want to publish relative to the root (main) directory is `Data_exploration_Rmarkdowns/name_of_file_to_publish.html`. An example of how to include this in the index file is shown below.

```md
 1. [BFISH Opaka Length Frequency Investigations]( https://pifscstockassessments.github.io/MHI_Bottomfish_2023/Data_exploration_Rmarkdowns/BFISH_Length_Comp.html)
 ```

### Suggested Dos and Don'ts 
Based on previous assessments and discussion among the SAP team, here are some recommended Dos and Don'ts for scripts and GitHub. These guidelines are meant to keep all files portable (no hardcoded paths), transparent, and easy to follow/implement. 

**DO:** 
- Split up data, scripts, and outputs (graphs, tables, etc.) into separate folders (subfolders can be created if necessary)
- Label script files clearly according to their purpose and order if used in a sequence (e.g. Catch_01_prep_data.R)
- use relative paths to files (e.g. "./Data/catch.csv")
- use RProject to create an R environment specifically for this project (check out the r package `here` for easily creating relative paths within a project)
- use the .gitignore file to keep artifact files, sensitive data, large files, or R environments from being pushed to the repo


**DON'T:**
- Don't write dates in the file names (e.g. Catch_01012020.R)
- Don't set working directory in the script
- Don't clear the environment `rm(ls())` within the script

## Dealing with Confidential Data  
Dealing with data that is confidential can be tricky when trying to make code publicly available. Some suggestions are to: 
- Create a separate folder that only contains confidential data and add this folder to the gitignore file.  
- Use a common filename structure (i.e. *_CON.csv) for all files that are confidential. This allows you to add that file name to the gitignore file, creating a second check that it will not be pushed to the repository as well as helping to identify that data set as confidential to other collaborators.
- Use R packages such as keyring to store credentials such as username and passwords that you do not want to write out in your code
- Use R packages such as dotenv to create variables for your environment that hides the name or link to confidential data
- Remember, the data is confidential but what you do to the data is not, so keep the initial prep script short and create a non-confidential version of the dataset that can be shared on the repo