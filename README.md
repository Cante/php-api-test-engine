Introduction
============

This bundle provides a class for a simple API Test. The tests run on the terminal using `run.php`.

<img alt="PHP API test engine screenshot" src="/../screenshots/php_api_test_engine_preview.png?raw=true" width="600" align="center"/>

How to use?
------------

In the folder you will find `run.php`. Run this file with given params to setup the test engine.

If you have written some tests. Then you can execute them with:

```bash
$ php run.php <api url> <test dir> <mock dir>
```

*Example:*

```bash
$ php run.php http://localhost:3000/api example/tests/ example/mocks/
```


How to write tests?
------------

First of all, all tests must contain in the same folder, sub folder are ignored.

###Create a simple test###

A test can contain one or more subtests.

```json
{
	"tests" : [
		{
			"name" : "Try to login with a wrong data",
			"path" : "/login",
			"method" : "POST",
			"request_params" : {
				"email" : "empty@me.com",
				"password" : "12345678"
			},
			"validation" : {
				"http_code" : 406,
				"response_params" : {
					"code" : 406,
					"message" : "$nn"
				}
			}
		},
		...
	]
}
```

**Options**

 - $nn means should be "not null"
 - *more options in the next version ...*


###Save variables###

To reuse output or created data you can save them to the globals (note: use unique keys!). Keypath should be a available path inside the request response.

```json
"save_global" : [
	{ "key" : "account_id", "keypath" : "account.id" }
]
```

To reuse the save variable you can do it easy with `{$account_id}`

```json
"name" : "Delete account with id {$account_id}",
"path" : "/accounts/{$account_id}",
```

###Using mocks###

If you have create mocks, you can easy load them by adding a string to the `request_params`. This example will load the mock `account.json`.

```json
"request_params" : "account",
```

You can also use mocks to validate the response values. For this add `mock` inside the validation.

```json
"validation" : {
	"http_code" : 200,
	"mock" : "account"
},
```

###Extended Header###

You can extend and overwrite the reuqest header using the `header` in the test set. The header variables will also listing to the global saved variables.

```json
"header" : {
    "Authorization" : "Bearer {$access_token}" 
}
```
