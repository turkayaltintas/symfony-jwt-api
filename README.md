# Smyfony 4 REST API

## Start

M.Türkay ALTINTAŞ
```
composer install or composer global update
```
Create a database ```symfonyshoping``` and username ```root```, password ```pass```

If .env not exist in project root folder then copy .env.dist to .env and update this connection url(DATABASE_URL)
```
DATABASE_URL=mysql://root:@localhost/symfonyshoping
```
Make migrations
```
php bin/console make:migration
php bin/console doctrine:migrations:migrate
php bin/console doctrine:fixtures:load --purge-with-truncate
```

If .htaccess
```
<IfModule mod_rewrite.c>
    RewriteEngine On
    
    RewriteCond %{ENV:REDIRECT_STATUS} ^$
    RewriteRule ^index\.php(/(.*)|$) %{CONTEXT_PREFIX}/$2 [R=301,L]
    
    RewriteCond %{REQUEST_FILENAME} -f
    RewriteRule .? - [L]
    RewriteCond %{REQUEST_FILENAME} !-f
    RewriteCond %{REQUEST_FILENAME} !-d
    RewriteRule ^cpv-\d+\/(.+)$ $1 [L]

    RewriteCond %{REQUEST_FILENAME} -f
    RewriteRule ^(.*)$ index.php [QSA,L]
    
    RewriteCond %{REQUEST_URI}::$1 ^(/.+)(.+)::\2$
    RewriteRule ^(.*) - [E=BASE:%1]
    RewriteRule .? %{ENV:BASE}index.php [L]

</IfModule>
<IfModule !mod_rewrite.c>
    <IfModule mod_alias.c>
        # When mod_rewrite is not available 
        RedirectMatch 302 ^/$ /index.php/
    </IfModule>
</IfModule>
```
Home Page
```
http://localhost/symfony-shopping-api/public/
```

Admin Panel
```
http://localhost/symfony-shopping-api/public/admin/product/list
Admin ->
admin@turkosoft.net
123456
```

API Endpoints ->
```
POST http://localhost/symfony-shopping-api/public/api/register
{
  "email":"demo@turkosoft.net",
  "plainPassword":"123456",
  "fullName":"Demo TurkoSoft"
}
```
Login to get token and that token could be used to access other api endpoints i.e order summary, save order.
```
POST http://localhost/symfony-shopping-api/public/api/login
{
  "email":"admin@turkosoft.net",
  "assword":"123456"
}

Response
{
"apikey": "6e847cd99e8a1e915001-1558878749"
}
```
Get Product list, There are two ways to append apikey, 1 Query string and 2 set 'X-AUTH-TOKEN' in request header
```
GET http://localhost/symfony-shopping-api/public/api/products?apikey=token
OR
GET http://localhost/symfony-shopping-api/public/api/products
Header name=X-AUTH-TOKEN
Header value= dbe7a82479ae4aea7f44-1558876797
```
Also can pass ```page``` and ```limit``` parameters into query string
```
GET http://localhost/symfony-shopping-api/public/api/products?page=2&limit=3
```
Customer order
```
POST http://localhost/symfony-shopping-api/public/api/customer/orders/summary?apikey=token
Content-Type: application/json
{
  "productsIdsArr":{
            "0":{"productId":1,"quantity":2},
            "1":{"productId":2,"quantity":2},
            "2":{"productId":4,"quantity":2}
  }
}
```
Customer order save
```
POST http://localhost/symfony-shopping-api/public/api/customer/orders/save?apikey=xyz-token
Content-Type: application/json
{
  "fullName":"Türkay ALTINTAŞ",
  "email":"turkay@turkosoft.net",
  "contactNumber":"+90",
  "postalCode":"06900",
  "shippingAddress":"Etimesgut...",
  "city":"Ankara",
  "country":"Türkiye",
  "customerNotes":"Hediye Paketi Olsun.",
  "productsIdsArr":{
            "0": {"productId":1,"quantity":1},
            "1": {"productId":2,"quantity":1},
            "2": {"productId":4,"quantity":1}
  }
}
```
Get single order
```
GET http://localhost/symfony-shopping-api/public/api/customer/orders/single/1?apikey=xyz-token
```
Get customer orders list
```
GET http://localhost/symfony-shopping-api/public/api/customer/orders?apikey=xyz-token
```
Some secure endpoints for admin user only. Admin apitoken will be mandatory because only Admin can perform these actions.
Add product
```
POST http://localhost/symfony-shopping-api/public/api/products
Content-Type: application/json
{
"title": "Mas",
"slug": "mass",
"price": 333,
"isDiscount": "No",
"discountType": null,
"discount": 0,
"isProductBundle": "No",
"sku": null,
"status": "Active",
"imageType": "Link",
"image": "https://turkosoft.net/400x300.png",
"description": null
}
```
Update product
```
PUT http://localhost/symfony-shopping-api/public/api/products/19
Content-Type: application/json
{
"title": "Masaa",
"slug": "masaa",
"price": 123,
"isDiscount": "No",
"discountType": null,
"discount": 0,
"isProductBundle": "No",
"sku": null,
"status": "Active",
"imageType": "Link",
"image": "https://turkosoft.net/400x300.png",
"description": null
}
````
Show single product
```
GET http://localhost/symfony-shopping-api/public/api/products/19
```
Delete product
```
DELETE http://localhost/symfony-shopping-api/public/api/products/19
```
Get those products which are not bundles so these can be display in dropdown for multi select when creating bundle
```
Get http://localhost/symfony-shopping-api/public/api/products-not-bundles
```
Add product bundle
```
POST http://localhost/symfony-shopping-api/public/api/product-bundles
Content-Type: application/json
{
"title": "Masalar",
"slug": "masa",
"price": 123,
"isDiscount": "No",
"discountType": null,
"discount": 0,
"isProductBundle": "Yes",
"sku": null,
"status": "Active",
"imageType": "Link",
"image": "https://turkosoft.net/400x300.png",
"description": null,
"productsArr":{"0":23,"1":12}
}
```
Update product bundle
```
PUT http://localhost/symfony-shopping-api/public/api/product-bundles/25
Content-Type: application/json
{
"title": "Masa",
"slug": "masa",
"price": 333,
"isDiscount": "No",
"discountType": null,
"discount": 0,
"isProductBundle": "Yes",
"sku": null,
"status": "Active",
"imageType": "Link",
"image": "https://turkosoft.net/400x300.png",
"description": "Awesome description",
"productsArr":{"0":3,"1":4}
}
```
Get product bundle
```
http://localhost/symfony-shopping-api/public/api/product-bundles/25
```
Delete product bundle
```
http://localhost/symfony-shopping-api/public/api/product-bundles/25
```
