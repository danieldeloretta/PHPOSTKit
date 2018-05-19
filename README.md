# PHPOSTKit (v1)
Still a rather uninteresting PHP implementation of OST Kit Alpha. This time updated for API v1. API v0 info is at the bottom of the page.

Currently supports the full [OSTKit v1 API](https://dev.ost.com/docs/api.html).

Again, the usage is fairly straight forward:
```php
require "path/to/config.php"; // this is where I keep my OST_API_KEY, OST_API_SECRET, OST_BASE_URL, and other stuff.
require "path/to/OST.class.php"; // or autoload it. whatever. i'm not your mum.

//create a user
$name = 'This was easy';
$create_user = OST::create_user($name);

//list some transactions
$transactions = 'transactionID1,transactionID2,transactionID3';
$list_options = ['id' => $transactions]; //see below for more about list options
$list_transactions = OST::list_transactions($list_options);

```

When calling the list methods, each one has its own set of options. You can view these options in the comments above the methods themselves. It is important to know you only need to provide the arguments you *need*. For example you want to list the first 50 actions that are `user_to_user`:

```php
$list_options = ['limit' => 50, 'kind' => 'user_to_user'];
$list_actions = OST::list_actions($list_options);
```
If you provide no `$list_options` then it will return the default list (10 entries, sorted by creation date in descending order).


All calls return an associative array, like so:
```php
/*
Array
(
    [success] => 1
    [data] => Array
        (
            [result_type] => user
            [user] => Array
                (
                    [id] => 56be7062-93a6-40b7-b97c-2802f96a10ed
                    [addresses] => Array
                        (
                            [0] => Array
                                (
                                    [0] => 1337
                                    [1] => 0xc4eD67D4Eb02190ff4dbC32985D14c4Ff4CAe41D
                                )

                        )

                    [name] => This was easy
                    [airdropped_tokens] => 0
                    [token_balance] => 0
                )

        )

)
*/
```

# KNOWN ISSUES
There are a few issues I have come across:

* `list_users` when you provide the optional filter `name` it is apparently ignored and returns the list as if the filter was not passed.
* `list_transactions` seems to stop returning transactions after you set `limit` to 83 or above.
* `list_transfers` when called with no options, doesn't return anything. Not even an error.






---- 

# PHPOSTKit (v0)
A rather uninteresting PHP implementation of OST Kit Alpha.

Currently supports the full OSTKit v0 API:

* Create user
* Edit user
* List users
* Airdrop users
* Airdrop status
* Create transaction type
* Edit a transaction type
* List transaction types
* Execute a transaction type
* Get status of an executed transaction (or multiple transactions at once)

**UPDATE 19 May 2018: Validation may have changed for various endpoints.**

Pretty straightforward usage:

```php
require_once "path/to/OSTv0.class.php"; // or autoload it. whatever. i'm not your mum.
$name = 'Foo Bar';
$user_uuid = \OST\OSTv0::create_user($name);


// to check a single tx status, ensure you pass the single uuid as a single element array like:
// $txs = ['a62fb7fe-ded8-4ecb-8489-4cb4c7d981ac'];
$txs = ['a62fb7fe-ded8-4ecb-8489-4cb4c7d981ac','1131d672-8859-42c6-99f0-2002dcaa2f6b'];
$tx_status = OST::tx_status($txs);

```

Don't forget to add in your API key, API secret, and base URL into your project. I defined them as a constant in a config file in my project.
