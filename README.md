# openai-codegen

A program to perform text/code generation tasks using the [openai models](https://beta.openai.com/docs/models/overview)

### Usage

    $ python3 program.py --help
    Usage: program.py [options]

    Options:
        -h, --help            show this help message and exit
        -p PROMPT, --prompt=PROMPT
                        The path to file containing the prompt text
        -m MODEL, --model=MODEL
                        The model to use. Options: 
                        1.  text-davinci-003 
                        2.  text-curie-001 
                        3.  text-babbage-001 
                        4.  text-ada-001
                        5.  text-davinci-002 
                        6.  text-davinci-001 
                        7.  davinci-instruct-beta 
                        8.  davinci 
                        9.  curie-instruct-beta 
                        10. curie 
                        11. babbage 
                        12. ada 
                        13. code-davinci-002 
                        14. code-cushman-001
        -o OUTPUT, --output=OUTPUT
                        The output file to store the result
        -t TEMPERATURE, --temperature=TEMPERATURE
                        The temperature of the model
        -l MAXLENGTH, --maxlength=MAXLENGTH
                        The maximum number of tokens in the response
        -k KEYFILE, --keyfile=KEYFILE
                        The file that holds the api key for openai


---
### Example 1: Generate a Fibonacci number algorithm that uses memoization

prompt

    # Python
    # a function that calculates a fibonacci number using memoization

command

    python3 program.py -p input.py -o fib.py

output

    # Python
    # a function that calculates a fibonacci number using memoization

    def fib(n, memo):
        if memo[n] is not None:
            return memo[n]
        if n == 1 or n == 2:
            result = 1
        else:
            result = fib(n-1, memo) + fib(n-2, memo)
        memo[n] = result
        return result

    def fib_memo(n):
        memo = [None] * (n + 1)
        return fib(n, memo)

    print(fib_memo(10))

---
### Example 2: Generate an AWS Cloudformation template for a dynamodb table

prompt

    Create a dynamodb cloudformation template for a dynamodb table
    The template should be in yaml format
    The table name should be company
    The primary key should be cid
    The read capacity should be 5
    The write capacity should be 5

command 

    python3 program.py -p input -o template.yaml -l 1024 -m text-davinci-003 -t 0.2

output

    Create a dynamodb cloudformation template for a dynamodb table
    The template should be in yaml format
    The table name should be company
    The primary key should be cid
    The read capacity should be 5
    The write capacity should be 5

    ---
    AWSTemplateFormatVersion: '2010-09-09'
    Resources:
    CompanyTable:
        Type: AWS::DynamoDB::Table
        Properties:
        TableName: company
        AttributeDefinitions:
            - AttributeName: cid
            AttributeType: S
        KeySchema:
            - AttributeName: cid
            KeyType: HASH
        ProvisionedThroughput:
            ReadCapacityUnits: 5
            WriteCapacityUnits: 5


---
### Example 3: Generate code to fetch prices for cryptocurrencies from coingecko.com

input.py

    # Python
    # Get the current price of BTC, ETH & DOGE from coingecko.com

command

    $ python3 program.py -p input.py -o output.py -l 512 t=0.2

output.py

    # Python
    # Get the current price of BTC, ETH & DOGE from coingecko.com
    #
    # Author:   @0rbz_
    # Date:     2020-04-01
    #
    # Usage:    python3 coingecko.py
    #
    # Requires: requests
    #
    # pip3 install requests

    import requests

    # Get the current price of BTC, ETH & DOGE from coingecko.com

    # BTC
    url = 'https://api.coingecko.com/api/v3/simple/price?ids=bitcoin&vs_currencies=usd'
    response = requests.get(url)
    data = response.json()
    btc_price = data['bitcoin']['usd']

    # ETH
    url = 'https://api.coingecko.com/api/v3/simple/price?ids=ethereum&vs_currencies=usd'
    response = requests.get(url)
    data = response.json()
    eth_price = data['ethereum']['usd']

    # DOGE
    url = 'https://api.coingecko.com/api/v3/simple/price?ids=dogecoin&vs_currencies=usd'
    response = requests.get(url)
    data = response.json()
    doge_price = data['dogecoin']['usd']

    # Print the results
    print('BTC: $' + str(btc_price))
    print('ETH: $' + str(eth_price))
    print('DOGE: $' + str(doge_price))

---
### Example 4: Generate unit tests for a memoized fibonacci function

prompt

    # Python
    # a function that calculates a fibonacci number using memoization

    def fib(n, memo):
        if n == 0 or n == 1:
            return n
        if memo[n] == 0:
            memo[n] = fib(n-1, memo) + fib(n-2, memo)
        return memo[n]

    def fibonacci(n):
        memo = [0] * (n+1)
        return fib(n, memo)

    print(fibonacci(100))

    # Unit test

command

    python3 program.py -p prompt -o output.py -l 256

output.py

    # Python
    # a function that calculates a fibonacci number using memoization

    def fib(n, memo):
        if n == 0 or n == 1:
            return n
        if memo[n] == 0:
            memo[n] = fib(n-1, memo) + fib(n-2, memo)
        return memo[n]

    def fibonacci(n):
        memo = [0] * (n+1)
        return fib(n, memo)

    print(fibonacci(100))

    # Unit tests
    import unittest

    class TestFibonacci(unittest.TestCase):
        def test_fibonacci(self):
            self.assertEqual(fibonacci(0), 0)
            self.assertEqual(fibonacci(1), 1)
            self.assertEqual(fibonacci(2), 1)
            self.assertEqual(fibonacci(3), 2)
            self.assertEqual(fibonacci(4), 3)
            self.assertEqual(fibonacci(5), 5)
            self.assertEqual(fibonacci(6), 8)
            self.assertEqual(fibonacci(7), 13)
            self.assertEqual(fibonacci(8), 21)
            self.assertEqual(fibonacci(9), 34)
            self.assertEqual(fibonacci(10), 55)
            self.assertEqual(fibonacci(11), 89)
            self.assertEqual(fibonacci(12), 144)
