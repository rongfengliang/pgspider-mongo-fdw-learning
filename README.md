# pgspider mongo fdw learning

## Usage

```code
CREATE EXTENSION mongo_fdw; 

CREATE SERVER mongo_server FOREIGN DATA WRAPPER mongo_fdw OPTIONS (address 'mongo', port '27017', authentication_database 'apps');

CREATE USER MAPPING FOR postgres SERVER mongo_server OPTIONS(username 'dalong', password 'dalong');

CREATE FOREIGN TABLE userapps(_id NAME,items int[],"userinfo.userage" int,"userinfo.username" text) SERVER mongo_server OPTIONS (database 'apps', collection 'myapps');

CREATE FOREIGN TABLE userapps_json(__doc jsonb) SERVER mongo_server OPTIONS (database 'apps', collection 'myapps');

select * from userapps;

select * from userapps_json;

SELECT __doc ::jsonb->> 'items' as __doc_info_items FROM userapps_json WHERE __doc ::jsonb->> 'items' is not null

 SELECT __doc ::jsonb->'userinfo' as usereinfo FROM userapps_json WHERE __doc ::jsonb->'userinfo'->>'username'='dalong';


mongodb docs:
/* 1 */
{
    "_id" : ObjectId("5e45fbd905d1df732b82b607"),
    "items" : [ 
        1, 
        3, 
        4
    ],
    "userinfo" : {
        "username" : "dalong",
        "userage" : 33
    }
}

/* 2 */
{
    "_id" : ObjectId("5e4601d905d1df732b82b6fa"),
    "items" : [ 
        11, 
        31, 
        41
    ],
    "userinfo" : {
        "username" : "dalong1",
        "userage" : 3
    }
}

/* 3 */
{
    "_id" : ObjectId("5e4601e905d1df732b82b6fe"),
    "items" : [ 
        12, 
        31, 
        41
    ],
    "userinfo" : {
        "username" : "dalong",
        "userage" : 3
    }
}
```