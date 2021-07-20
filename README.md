# Introduction 
This is a sample that illustrates StepZen's GraphQL API and MySQL capabilities. It uses the [Everbase API](https://www.everbase.co/docs) and a MySQL database deployed on [Railway]().
This way, you can get information from both Everbase and your database in a single query, so that your frontend can eliminate unnecessary client logic. 
It's also super easy to get graphql and mysql schemas spun up using the StepZen CLI-- to learn how using this example, see [our blog post](#no-blog-post-yet).

## Getting Started
You'll need to create a [StepZen account](https://stepzen.com/request-invite) first. Once you've got that set up, [git clone](https://www.atlassian.com/git/tutorials/setting-up-a-repository/git-clone) this repository onto your machine and open the working directory.

You'll also need to sign up for [Everbase](https://www.everbase.co/) to get your API key.

Use these instruction to deploy a [database on Railway](https://gist.github.com/ajcwebdev/8599295373c092f30fce9968eaf48635).
Note that you will modify these instructions-- instead of runnning 
```sql
CREATE TABLE test_table (Test text);
INSERT INTO test_table VALUES ('Hello'), ('Goodbye');
show tables;
```
You will run
```sql
CREATE TABLE countries (
	country_id varchar(255),
	country_name varchar(255),
	isoCode varchar(255),
	GDPUSD int
);

INSERT INTO countries (country_id, country_name, isoCode, GDPUSD)
VALUES (
	"Q889",
	"Afghanistan",
	"AFN",
	19.29
),
(
	"Q953",
	"Zambia",
	"ZMW",
	23.31
),
(
	"Q664",
	"New Zealand",
	"NZD",
	206.9
	),
(
	"Q1029",
	"Mozambique",
	"MZN",
	15.29
),
(
	5,
	"Switzerland",
	"CHF",
	703.1
),
(
	"Q222",
	"Albania",
	"ALL",
	15.28
),
(
	"Q155",
	"Brazil",
	"BRL",
	1840
),
(
	"Q970",
	"Comoros",
	"KMF",
	1.16
),
(
	"Q33",
	"Finland",
	"EUR",
	269.3
),
(
	"Q142",
	"France",
	"EUR",
	2716
	
),
(
	"Q41",
	"Greece",
	"EUR",
	209.9 
),
(
	"Q117",
	"Ghana",
	"GHS",
	66.98
),
(
	"Q766",
	"Jamaica",
	"JMD",
	16.46
);
```

## Your configuration

Make a file in your working directory called `config.yaml`

```yaml
configurationset:
  - configuration:
      name: api_everbase_co_graphql_config
      Authorization: {{EVERBASE_AUTH_KEY_HERE}}
  - configuration:
      name: mysql_config
      dsn: {{USERNAME HERE}}:{{PASSWORD HERE}}@tcp({{HOST_HERE}}:{{PORT_HERE}})/railway
```
You'll use your Everbase authorization for `EVERBASE_AUTH_KEY_HERE`, 
and then your railway username (probably 'root') for `USERNAME_HERE`, 
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

<img alt="screenshot" src="https://i.ibb.co/QPHT6sG/Screen-Shot-2021-07-16-at-12-17-45-PM.png" width="800" />

In the middle pane, copy and paste:

```graphql
query MyQuery {
  GDPByCountryName(name: "Albania") {
    GDPUSD
    stepzen_timeZones {
      id
      name
      offset
    }
  }
}
```

and you'll get the response:

```json
{
  "data": {
    "GDPByCountryName": {
      "GDPUSD": 15,
      "stepzen_timeZones": [
        {
          "id": "Q7023",
          "name": "UTC+08:30",
          "offset": 8.5
        },
        {
          "id": "Q7041",
          "name": "UTC+09:00",
          "offset": 9
        },
        {
          "id": "Q48916190",
          "name": "UTC+06:55:25",
          "offset": 6.923611
        },
        ....etc
```

## Learn More
You can learn more in the [StepZen documentation](https://stepzen.com/docs).
