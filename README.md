# btcpay-python

## Install
```shell
pip3 install btcpay-python
```
If you were a user of the prior unofficial client library for Python, you would need to uninstall it first:
```shell
pip3 uninstall btcpay
pip3 install btcpay-python
```
This library is fully backward compatibile with the prior unofficial library; no code changes are needed.

## The "easy method" to create a new BTCPay client
* On BTCPay server > shop > access tokens > create new token, copy pairing code.
* Then use that code in the below Python code:
```python
from btcpay import BTCPayClient

client = BTCPayClient.create_client(host='https://btcpay.example.com', code=<pairing-code>)
```

## Uses for the client object you just created above

# Get rates
```python
client.get_rates()
```


# Create specific rate
```python
client.get_rate('USD')
```


# Create invoice
See bitpay api documentation: https://bitpay.com/api#resource-Invoices
```python
client.create_invoice({"price": 20, "currency": "USD"})
```


# Get invoice
```python
client.get_invoice(<invoice-id>)
```

## Storing the client object for later

You do not need to store any tokens or private keys. Simply `pickle` the client object and save it to your persistent storage method (Redis, SQLAlchemy, etc). Pull it from persistent storage later, unpickle it, and perform any of the methods on it which you may need.


## Creating a client the manual way (not necessary if you used the 'easy' method above)
* Generate and save private key:
```python
import btcpay.crypto
privkey = btcpay.crypto.generate_privkey()
```
* Create client:
```python
from btcpay import BTCPayClient
client = BTCPayClient(host='http://hostname', pem=privkey)
```
* On BTCPay server > shop > access tokens > create new token, copy pairing code:
* Pair client to server and save returned token:
```python
client.pair_client(<pairing-code>)
>>> {'merchant': "xdr9vw3v5wc0w90859v45"}
```
* Recreate client:
```python
client = BTCPayClient(
    host='http://hostname',
    pem=privkey,
    tokens={'merchant': "xdr9vw3v5wc0w90859v45"}
)
```
