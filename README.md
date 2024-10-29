# Outline

* An explanation of how you scraped the data
* The most interesting and surprising fact you found after analyzing the the data
* An actionable recommendation for developers based on your analysis


## How to scrape GitHub data?

### Create GitHub acces token

First we need a token to access GitHub API ( mainly required to avoid rate-limiting ).

 * First goto your GitHub account settings: https://github.com/settings/profile
 * Then goto Developer Settings
 * Then goto Personal Access Tokens
 * Then goto Tokens (classic)

Here create a new token and keep it in a .env file under the root folder of your project.

Contents of .env file

```
GITHUB_TOKEN=<YOUR_TOKEN_HERE>
```

### User API endpoints to fetch User data

You can use `dotenv` python package to load the `.env` file and then hit following API endpoint with correct headers using any method. We used Python `requests` library:

```
url = f"https://api.github.com/search/users?q=location:{location}+followers:>{min_followers}&per_page=100"
headers = {"Accept": "application/vnd.github+json", "Authorization": f"Bearer {os.getenv("GITHUB_TOKEN")}"}
response = requests.get(url, headers=headers)
```

Response will contain 100 users at a time which we can navigate page by page using Response header called `Link`. This has navigation information which we can capture by parsing and extracting the next page url.

### User API endpoints to fetch User Profile data

For each user found above we will hit another endpoint: 
```
url = f"https://api.github.com/users/{username}"
```

### Repositories API endpoints to fetch User public repositories

For each user found above we will hit another repository endpoint: 

```
url = f"https://api.github.com/users/{username}/repos"
```

### Processing the data

We use user profile data to generate `users.csv`

We only select 500 public repositories for each user order by `pushed_at` timestamp in descending order.
Then we generate `repositories.csv` file.
 

All of these steps are present in the accompanying Jupyter notebook code.

## Most interesting and surprising fact found after analyzing the data



## An actionable recommendation for developers based on the analysis

