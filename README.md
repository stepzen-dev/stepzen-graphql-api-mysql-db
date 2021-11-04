# Introduction 
This is a sample that illustrates StepZen's GraphQL API and MySQL capabilities. It uses a [countries API](https://github.com/trevorblades/countries) and a MySQL database deployed on [Railway](https://railway.app/).
This way, you can get information from both the countries API and your database in a single query, so that your frontend can eliminate unnecessary client logic. 
It's also super easy to get graphql and mysql schemas spun up using the StepZen CLI-- to learn how using this example, see [our blog post](#no-blog-post-yet).

## Contribution

If you'd like to contribute, please view our [guidelines](https://github.com/stepzen-samples/stepzen-graphql-api-mysql-db/blob/main/CONTRIBUTING.md) and [code of conduct](https://github.com/stepzen-samples/stepzen-graphql-api-mysql-db/blob/main/CODE_OF_CONDUCT.md) first.

Here's our video walkthrough as well. In it, you'll [learn the steps for making a PR](https://www.youtube.com/watch?v=4B6xVyEc_CY&t=1285s) and the overall goals for our repos. It's for Hacktoberfest, but the process applies to our repos year-round. 

If you have any more questions about working with the sample, hit us up on [Discord](https://discord.com/invite/9k2VdPn2FR).

## Getting Started
You'll need to create a [StepZen account](https://stepzen.com/request-invite) first. Once you've got that set up, [git clone](https://www.atlassian.com/git/tutorials/setting-up-a-repository/git-clone) this repository onto your machine and open the working directory.

Use these instructions to deploy a [database on Railway](https://gist.github.com/ajcwebdev/8599295373c092f30fce9968eaf48635).
Note that you will modify these instructions-- instead of runnning 
```sql
CREATE TABLE test_table (Test text);
INSERT INTO test_table VALUES ('Hello'), ('Goodbye');
show tables;
```
You will run
```sql
mysql> CREATE TABLE countries (
    -> code varchar(255)
    -> name varchar(255)
    -> GDP int );
    
INSERT INTO countries (code, name, GDP)
VALUES (
	'AF', 
	'Afghanistan',
 	19
),
(
	'ZM', 
	'Zambia', 
 	23
),
(
	'NZ', 
	'New Zealand',
	207
	),
(
	'MZ', 
	'Mozambique', 
	15
),
(
	'CH', 
	'Switzerland', 
	703
);
```

## Your configuration

Make a file in your working directory called `config.yaml`

```yaml
configurationset:
  - configuration:
      name: mysql_config
      dsn: {{USERNAME HERE}}:{{PASSWORD HERE}}@tcp({{HOST_HERE}}:{{PORT_HERE}})/railway
```
You'll use your railway username (probably 'root') for `USERNAME_HERE`, 
your dsn password for `PASSWORD_HERE`,
your railway hostname for `HOST_HERE`,
and your port for `PORT HERE`.

> Double-check that your `config.yaml` is in your `gitignore` file. 

## The StepZen CLI

Open your terminal and [install the StepZen CLI](https://stepzen.com/docs/quick-start). 

After you've followed the prompts (you can accept the suggested endpoint name-- in my case it was `api/happy-bunny`) and installed the CLI, run `stepzen start`.
A message similar to this will display:

```bash
Watching ~/gql-github for GraphQL changes
http://localhost:5000/api/happy-bunny
File changed: /Users/luciacerchie/gql-github/.git/config
Deploying to StepZen...... done
Successfully deployed api/happy-bunny at 9:00:07 AM
```

## The StepZen Explorer

You'll see your StepZen Explorer pop up on your localhost:5000 address:

<img width="1361" alt="Screen Shot 2021-07-20 at 2 15 34 PM" src="https://user-images.githubusercontent.com/54046179/126396456-9f3d977a-d12d-484e-a90f-073f869d4e74.png">


In the middle pane, copy and paste:

```graphql
query MyQuery {
  getCountryByCodeMySQL(code: "CH") {
    GDP
    code
    name
    sz_Country {
      capital
      code
    }
  }
}

```

and you'll get the response:

```json
{
  "data": {
    "getCountryByCodeMySQL": {
      "GDP": 703,
      "code": "CH",
      "name": "Switzerland",
      "sz_Country": {
        "capital": "Bern",
        "code": "CH"
      }
    }
  }
}
```

## Learn More
You can learn more in the [StepZen documentation](https://stepzen.com/docs).
