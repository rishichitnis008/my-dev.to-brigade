  <img src="https://media.dev.to/cdn-cgi/image/width=1000,height=420,fit=cover,gravity=auto,format=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F37pob2c87h85aonjmgz5.png" alt="Cover Image" />
  <hr />
  
  # Adding Profile README Stats to Your Github Profile README
  
  **Tags:** `github`, `githubactions`, `beginners`, `tutorial`

  **Published At:** 8/3/2024, 5:56:41 PM

  **URL:** [https://dev.to/rishichitnis008/adding-profile-readme-stats-to-your-github-profile-readme-1cc2](https://dev.to/rishichitnis008/adding-profile-readme-stats-to-your-github-profile-readme-1cc2)

  <hr />
  Have You ever looked at a Github Profile and saw Github Profile README Stats on it?

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/bb0c95tf24lxndunxsyg.png)

Well I'm going to walk you through the Steps of How you can have it also.

### Step 1: You Will Need a Github Token:

You will need a Github Personal Access Token with the Following scops

`repo` and `read:user`

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/noqsen4i6c3hj9pgyyf2.png)

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/clcypu9voki6e7co47zy.png)


### Step 2: You will need to add it to Your Repository's Secrets

You will need to go to your Repo's Settings, Click on `Secrets and Variables` and then Click on `New Secret` and Name it Anything you want like `USER_TOKEN` and add the Personal Access Token from the Previous Step.

### Step 3: Create a Template file:

You will Need a Template file for this to work.

Add the Following code:

```
Joined Github **{{ ACCOUNT_AGE }}** years ago.

Since then I pushed **{{ COMMITS }}** commits, opened **{{ ISSUES }}** issues, submitted **{{ PULL_REQUESTS }}** pull requests, received **{{ STARS }}** stars across **{{ REPOSITORIES }}** personal projects and contributed to **{{ REPOSITORIES_CONTRIBUTED_TO }}** public repositories.

Most used languages across my projects:

{{ LANGUAGE_TEMPLATE_START }}
![{{LANGUAGE_NAME}}](https://img.shields.io/static/v1?style=flat-square&label=%E2%A0%80&color=555&labelColor={{LANGUAGE_COLOR:uri}}&message={{LANGUAGE_NAME:uri}}%EF%B8%B1{{LANGUAGE_PERCENT:uri}}%25)
{{ LANGUAGE_TEMPLATE_END }}

<p align="right"><sub>Generated using <a href="https://github.com/marketplace/actions/profile-readme-stats">teoxoy/profile-readme-stats</a></sub></p>
```


### Step 4: Create The Github Action:

Go to the Actions tab, Click on `set up new workflow yourself`, and in the `.github/workflows` directory name your file `stats.yml` and add the following code:

```
on:
  workflow_dispatch:
  schedule:
    - cron: '0 */6 * * *' # every 6 hours
  push:
    branches:
      - master
jobs:
  publish:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v4
      with:
        fetch-depth: 0
    - name: Generate README.md
      uses: teoxoy/profile-readme-stats@v3
      with:
        token: ${{ secrets.USER_TOKEN }}
    - name: Update README.md
      run: |
        if [[ "$(git status --porcelain)" != "" ]]; then
        git config user.name github-actions[bot]
        git config user.email 41898282+github-actions[bot]@users.noreply.github.com
        git add .
        git commit -m "Update README"
        git push
       fi
```

### Step 5: Run The Workflow

You should get the following output when you run the workflow, if you don't please feel free to look at [My Profile](https://github.com/rishichitnis008/rishichitnis008/blob/main/.github/workflows/profile-stats.yml): 


![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/tv39xkiaeemhbo1hzyvq.png)


And Boom, Your Profile Stats will automatcally get added every few hours.

See you next time

Bye
    
  