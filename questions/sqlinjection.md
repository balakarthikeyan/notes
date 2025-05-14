# Cross-site Scripting (XSS)
Cross-site Scripting (XSS) is a client-side code injection attack. The attacker aims to execute malicious scripts in a web browser of the victim by including malicious code in a legitimate web page or web application.

## Types of Injection Attacks

### Code injection
The attacker injects application code written in the application language. This code may be used to execute operating system commands with the privileges of the user who is running the web application. In advanced cases, the attacker may exploit additional privilege escalation vulnerabilities, which may lead to full web server compromise.

`Potential impact :` Full system compromise

### CRLF injection
The attacker injects an unexpected CRLF (Carriage Return and Line Feed) character sequence. This sequence is used to split an HTTP response header and write arbitrary contents to the response body. This attack may be combined with Cross-site Scripting (XSS).

`Potential impact :` Cross-site Scripting (XSS)

### Cross-site Scripting (XSS)
The attacker injects an arbitrary script (usually in JavaScript) into a legitimate website or web application. This script is then executed inside the victim’s browser.
> `Potential impact : `
- Account impersonation
- Defacement
- Run arbitrary JavaScript in the victim’s browser

### Email Header Injection
This attack is very similar to CRLF injections. The attacker sends IMAP/SMTP commands to a mail server that is not directly available via a web application.
> `Potential impact : `
- Spam relay
- Information disclosure

### Host Header Injection
The attacker abuses the implicit trust of the HTTP Host header to poison password-reset functionality and web caches.
> `Potential impact :` 
- Password-reset poisoning
- Cache poisoning

### LDAP Injection
The attacker injects LDAP (Lightweight Directory Access Protocol) statements to execute arbitrary LDAP commands. They can gain permissions and modify the contents of the LDAP tree.
> `Potential impact : `
- Authentication bypass
- Privilege escalation
- Information disclosure

### OS Command Injection
The attacker injects operating system commands with the privileges of the user who is running the web application. In advanced cases, the attacker may exploit additional privilege escalation vulnerabilities, which may lead to full system compromise.

`Potential impact : `Full system compromise

### XPath injection
The attacker injects data into an application to execute crafted XPath queries. They can use them to access unauthorized data and bypass authentication.
> `Potential impact : `
- Information disclosure
- Authentication bypass

### SQL Injection (SQLi)
The attacker injects SQL statements that can read or modify database data. In the case of advanced SQL Injection attacks, the attacker can use SQL codes inserted into input fields of a web application, exploiting vulnerabilities in the application's database layer. and even execute OS commands. This may lead to manipulate or retrieve data, modify database entries, and even execute administrative operations. 
> `Potential impact : `
- Authentication bypass
- Information disclosure
- Data loss
- Sensitive data theft
- Loss of data integrity
- Denial of service
- Full system compromise.

```php
if (isset($_GET['id'])){
    $id = $_GET['id'];

    /* Setup the connection to the database */
    $mysqli = new mysqli('localhost', 'dbuser', 'dbpasswd', 'sql_injection_example');

    /* Check connection before executing the SQL query */
    if ($mysqli->connect_errno) {
        printf("Connect failed: %s\n", $mysqli->connect_error);
        exit();
    }

    /* SQL query vulnerable to SQL injection */
    $sql = "SELECT username
    FROM users
    WHERE id = $id";

    /* Select queries return a result */
    if ($result = $mysqli->query($sql)) {
        while($obj = $result->fetch_object()){
            print($obj->username);
        }
    } elseif($mysqli->error){
        print($mysqli->error);
    }
}

/**
 * Check if the 'id' GET variable is set
 * Example - http://localhost/?id=1
 */
> johnsmith

/**
 * The following is an example of a malicious HTTP request that could be made to the vulnerable application above.
 */
http://localhost/?id=-1 UNION SELECT password FROM users where id=1
> $2a$10$SakFH.Eatq3QnknC1j1uo.rjM4KIYn.o8gPb6Y2YBnNNNY.61mR9K

if (isset($_GET['id'])){
    $id = $_GET['id'];
    /**
     * Validate data before it enters the database. In this case, we need to check that
     * the value of the 'id' GET parameter is numeric
     */
    if ( is_numeric($id) == true){
        // Check connection before executing the SQL query
        try{ 
            /**
             * Setup the connection to the database This is usually called a database handle (dbh)
             */
            $dbh = new PDO('mysql:host=localhost;dbname=sql_injection_example', 'dbuser', 'dbpasswd');
            
            /**
             * Use PDO::ERRMODE_EXCEPTION, to capture errors and write them to
             * a log file for later inspection instead of printing them to the screen.
             */
            $dbh->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);
            
            /**
             * Before executing, prepare statements by binding parameters.
             * Bind validated user input (in this case, the value of $id) to the
             * SQL statement before sending it to the database server.
             *
             * This fixes the SQL injection vulnerability.
             */
            $q = "SELECT username FROM users WHERE id = :id";
            // Prepare the SQL query string.
            $sth = $dbh->prepare($q);
            // Bind parameters to statement variables.
            $sth->bindParam(':id', $id);
            // Execute statement.
            $sth->execute();
            // Set fetch mode to FETCH_ASSOC to return an array indexed by column name.
            $sth->setFetchMode(PDO::FETCH_ASSOC);
            // Fetch result.
            $result = $sth->fetchColumn();
            /**
             * HTML encode our result using htmlentities() to prevent stored XSS and print the result to the page
             */
            print( htmlentities($result) );
            //Close the connection to the database.
            $dbh = null;
            // // Prepare an SQL statement
            // $stmt = $pdo->prepare("INSERT INTO table (column) VALUES (:user_input)");
            // // Bind parameters
            // $stmt->bindParam(':user_input', $user_input);
        } catch(PDOException $e){
            /**
             * You can log PDO exceptions to PHP's system logger, using the log engine of the operating system
             *
             * For more logging options visit http://php.net/manual/en/function.error-log.php
             */
            error_log('PDOException - ' . $e->getMessage(), 0);
            /**
             * Stop executing, return an Internal Server Error HTTP status code (500), and display an error
             */
            http_response_code(500);
            die('Error establishing connection with database');
        }
    } else{
        /**
         * If the value of the 'id' GET parameter is not numeric, 
         * stop executing, return a 'Bad request' HTTP status code (400), and display an error
         */
        http_response_code(400);
        die('Error processing bad or malformed request');
    }
}
```