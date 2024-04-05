<!-- // cSpell:enable -->
# ECS - PHP/SQLite | Vue JS

### Convert Ryan's ECS (TipleR) system to run on a Linux/Apache web server, using VueJS and PHP.

Ryan's current system uses Django (Python web framework).

REST API requests are made from the `VueJS` frontend to the backend:

```
ip:port/resends
ip:port/resends/get_order_count/2023-04-05/2023-04-12
ip:port/resends/get_warehouse_count/packer/2023-04-05/2023-04-12
ip:port/refunds/get_void_daily/2023-04-05/2023-04-12
...
```

A JSON response is returned:

E.g.
```json
[
   {
      "created": "2022-03-28",
      "notes": "Missing 60 pots. Resend PI:52 PA:0 GA",
      "dor": 0,
      "reason_id": 3,
      "user_id": 4,
      "room_id": 1,
      "packer_id": 1,
      "picker_id": 16,
      "option_id": 5,
      "order_id": "167470-a"
   },
    ...
```

The `index.php` script emulates the Django backend. It has a switch statement with multiple SQL statements for each case:
```php
switch ($func) {
   case 'get_order_count':
      $sql = "...";
      break;
   case 'get_reason_count':
      $sql = "...";
      break;
   case 'get_reason_daily':
      $sql = "...";
      break;
   ...
}
```
`index.php` references the `db.sqlite3` database, so this is obviously required.

Running the script:
```
$ cd /var/www/html/ecs_php_db_query/
$ php -S 127.0.0.1:8000
```
The script can now be queried in the web browser.  
E.g `http://127.0.0.1:8000/resends/?page=3`
```json
[
    {
        "created": "2022-03-31",
        "notes": "Send 2kg of Garden Lime to replace lost in transit. GAS",
        "dor": 0,
        "reason_id": 3,
        "user_id": 4,
        "room_id": 3,
        "packer_id": 1,
        "picker_id": 1,
        "option_id": 5,
        "order_id": "167816-a"
    },
    ...
```
`http://127.0.0.1:8000/returns/get_order_count/2023-04-05/2023-04-12`
```
[
    {
        "order_total": 102,
        "created": "2023-04-08"
    },
    {
        "order_total": 1,
        "created": "2023-04-11"
    }
]
```
etc.

Note.  
The URL format of the requests requires module rewrite to be enabled and the .htaccess file.

Refer to 'Setup mod write' comment in `index.php`.







<!--
# SECTION LINKS
* [View Orders](#view-orders)
* [Scan Barcodes](#scan-barcodes)

# Above link to: ## View Orders, ## Scan Barcodes
_ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _

![Image of Img_Name](docs/imgs/img_name.png)

_ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _

*ITALIC*
**BOLD**
***BOLD & ITALIC***
_ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _

# LIST
* item 1
* item 2
* item 3
_ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _

# DISPLAY CODE
`CODE_IN_BACKTICKS`

```php
if (isset($_GET['id'])) {
    //CODE_HERE
}
```

```html
<tr>
    <td class="tick fvis-status-marked">...</td>
</tr>
```

```js
let tickIDs = [];
```
_ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _

# TABLES

heading 1 | heading 2
| --- | --- |
row1, col1 | row1, col2
row2, col1 | row2, col2

-->

