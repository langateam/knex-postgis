# knex-postgis

Extension for use postgis functions in [knex](http://knexjs.org) SQL query builder.



## Example
This example show the sql generated by the extension.


```js
var knex = require('knex')({
  dialect: 'postgres'
});

// install postgis functions in knex.postgis;
var st = require('../lib/index')(knex);

// insert a point
var sql1 = knex.insert({
  id: 1,
  geom: st.geomFromText('Point(0 0)', 4326)
}).into('points').toString();
console.log(sql1);
// insert into "points" ("geom", "id") values (ST_geomFromText('Point(0 0)'), '1')

// find all points return point in wkt format
var sql2 = knex.select('id', st.asText('geom')).from('points').toString();
console.log(sql2);
// select "id", ST_asText("geom") as "geom" from "points"

// all methods support alias
var sql3 = knex.select('id', st.asText(st.centroid('geom')).as('centroid')).from('geometries').toString();
console.log(sql3);
// select "id", ST_asText(ST_centroid("geom")) as "centroid" from "geometries"

```

## currently supported functions

- asText(column)
- asEWKT(column)
- centroid(geom)
- intersects(geom1, geom2)
- geomFromText(geom, srid)
