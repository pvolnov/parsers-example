Yandex Market Parser bitech
====================


### How it works
The bot is written in python 3 using Selenium. During its work it connects to selnoid on the server and in several threads opens the browser window one by one, makes a search query, then opens the product pages and collects information about the characteristics of the products from them. All information is unloaded into the database, on restart continues where it left off. The script `task_upload.py` is used to control the parser: viewing the loading status, adding jobs, and unloading data. 

### Preconfiguration
In the `configs.py` file we must update some of the constants:
``python
db_name = '[Postges database name]''.
db_login = '[login to connect Postges]''
db_password = '[Postges password]''
db_host = '[IP of Postges database]''
db_port = 5432

db_ms_name = '[MSsql database name]''.
db_ms_login = '[login to connect MSsql]''
db_ms_password = '[MSsql password]''
db_ms_host = '[MSsql database IP]''.

RU_CAPTCHA_APY_KEY = '[token from https://rucaptcha.com/ service necessary to pass captcha on site]''.
SELENOID_ADRESS = "[address to connect to selenoid example: http://127.0.0.1:4444/wd/hub]"
```
[Setup instructions](https://4te.me/post/selenium-docker/) on the ``selenoid`` server

[Getting RuCaptchaApyKey](https://rucaptcha.com/auth/register)

### Work with the parser
0.* If you are running the parser for the first time with your database, run `python3 task_upload.py --init` to create the tables you need to work with.
1. Put in the table `tasks` on the mssql server a list of original names in the `name` column.
2. Execute `python3 task_upload.py -u` to add all names to the search task 
3. Run the parser by running `python3 parser.py` and wait for parsing to finish, it is recommended to run it once in the background, if there are no tasks it will go into sleep mode
4. You can check the status of parsing by running the command `python3 task_upload.py -s `
5. When the parsing is finished, run the command `python3 task_upload.py -e ` to unload the parsing results. All the products whose name was in the passed file will be unloaded. The file `result.csv` will be generated and also lines will be added to the table `result` on the server MsSQl


#### Run on the server 
To set up the server run

``bash
sudo apt-get update
sudo apt-get upgrade
sudo apt-get install git python3-pip
git clone https://github.com/Neafiol/parsers-example
cd parsers-example
bash ./install.sh
```
