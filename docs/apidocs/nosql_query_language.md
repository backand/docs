#JSON Query Language

The language is inspired by MongoDB.

A query consists of these parts:

1. fields to be extracted 
2. table
3. expression for filtering the rows
4. groupby - group by fields
5. aggregate functions to be applied to columns in fields
6. orderby - order by fields
7. limit - limit by integer

Only the table and the expression are mandatory


Corresponding to the SQL query:

    SELECT fields with aggregation
    FROM table
    WHERE expression
    GROUP BY groupby
    ORDER BY orderby
    LIMIT limit


We write a query is a JSON object of the form:

    { 
        object: String, 
        q: Expression, 
        fields: Array of String, 
        groupBy: Array of String, 
        aggregation: Object mapping fields to aggregate functions 
    }

The  shortest query omitting fields is,

    { object: String, q: Expression }

in which case it becomes:

    SELECT *
    FROM table
    WHERE query

# Examples

A simple query to retrieve the name and salary of all employees in position of "Sales Manager" is:

    { object: "employees", q: { position : "Sales Manager"  }, fields: ["name", "salary"] }

Queries can compare fields to constants using all the conventional comparison operators. 

To retrieve all fields of employees under the age of 25, 

    { object: "employees", q: { age : { $lt : 25 } }  } 

# Expressions

More generally, an expression can be either an AND expression or an OR expression or a UNION query

* an AND expression is a conjunction of conditions on fields. An AND expression is a JSON of the form `{ A: condition, B: condition, ... }`

To test employees on age, position, and city, use the and expression:

    { position: "Sales Manager", age : { $lt : 25 }, city: "Boston" }

* an OR expression is a disjunction of conditions, `{ $or: [ Expression1, Expression2, ...   ] }` 

To find all departments having more than 30 employees, or located in "Los Angeles", 

    { $or: [ { num_employees: { $gt: 30 } }, { location: "Lost Angeles" }  ]  }

* a UNION query is a union of query: `{ $union: [ Query1, Query2, ...   ] }`

    {
        "$union":   [
            {
                "object" : "Employees",
                "q" : {
                    "$or" : [
                        {
                            "Budget" : {
                                "$gt" : 20
                            }
                        },
                        {
                            "Location" : { 
                                "$like" :  "Tel Aviv"
                            }
                        }
                    ]
                },
                fields: ["Location", "country"]
            },
            {
                "object" : "Person",
                "q" : {
                    "name": "john"
                },
                fields: ["City", "country"],
                limit: 11
            }
        ]
    }

# Conditions on Fields

Generally, a condition on a field is a predicate can can do one of the following:

1. Test equality of field to a constant value, e.g.  `{ A: 6 }`. Is `A` equal to 6?
2. Comparison of a field using a comparison operator, e.g. `{ A: { $gt: 8 }}`. Is `A` greater than 8? The set of comparison operators is quite extensive and includes: `$lte, $lt, $gte, $gt, $eq, $neq, $not`
3. Test if the value of the field is IN  or NOT IN the result of a subquery.
4. Test for the negation of a comparison. For example, to test if the location field is not Boston, we can do:

    { $not: { location : "Boston" }}

# Sub Queries

If we have a subquery that retrieves the department id of each department in New York, 

    { object: "department", "q": { "city" : "New York" }, "fields" : ["id"]}

we test a field dept_id with respect to the result of the subquery as:

    { dept_id: { $in: {  
        { object: "department", "q": { "city" : "New York" }, "fields" : ["id"]}
    }  }}

The result of the subquery should retrieve a single field for this to work.

We use the subquery in a query retrieving all employees of departments located in New York, where the `deptId` field is a reference from the `employees` table to the `department` table, as:

    { 
        object: "employees", 
        "q" : { 
            "deptId" : { 
                $in: { 
                    object: "department", 
                    "q": { 
                        "city" : "New York" 
                    }, 
                    "fields" : ["id"] 
                }
            }

        } 
    } 

A more complicated query is to retrieve all employees whose department is located in New York such that the employee is located in Boston. We use an AND expression on the two conditions:
 
    { 
        object: "employees", 
        "q" : { 
            deptId": 
            { 
                $in: { 
                    object: "department", 
                    q: { 
                        "city" : "New York" 
                    }, 
                    fields : ["id"] 
                }
            },
            location: "Boston"
        } 
    }


# Conditions on Fields


Formally, a condition on a field is a key-value expression of the form: 
     
      Key : ValueExpression

* Key - name of field
* ValueExpression - which has one of the following forms:

    1. Constant - is the field value equal to the constant
    2. Comparison with a comparison operator to a constant 
    3. Inclusion or exclusion in result of a sub query
    3. Negation of another comparison

Negation may sometimes be swapped for comparison. For example, to test if the location field is not equal to Paris, we can

use negation:

    { $not: { location : "Paris" }}


use a not equal operator: 

    { location: { $neq: "Paris" }}

# Group By Queries

A group by query aggregates on fields, and then applies aggregation operators to some of the fields. For instance, to group by `Country`, and then concat the `Location` field:

    {
        "object" : "Employees",
        "q" : {
            "$or" : [
                {
                    "Budget" : {
                        "$gt" : 20
                    }
                },
                {
                    "Location" : { 
                        "$like" :  "Tel Aviv"
                    }
                }
            ]
        },
        fields: ["Location", "Country"],
        order: [["Budget", "desc"]],
        groupBy: ["Country"],
        aggregate: {
            Location: "$concat"
        }
    }

# Algorithm to Generate SQL from JSON Queries

The algorithm transforms a JSON to SQL by top-down transformation. 

## Usage

    transformJson(json, sqlSchema, isFilter, callback) 

The parameters are:

1. `json` - JSON query or filter
2. `sqlSchema` - JSON schema of database
3. `isFilter` - boolean if `json` is filter
4. `callback` - `function(err, result)`

The result is a structure with fields:

    {
        str: <SQL statement for query>,
        select: <select clause>,
        from: <from clause>,
        where: <where clause>,
        group: <group by clause>,
        order: <order by clause>,
        limit: <limit clause>     
    }

## Escaping

All constants appearing in the JSON query are escaped when transformed to SQL.

## Filters

For the case of filter, the query can include variables. Variables take the form of:

    {{<variable name>}}

and should be enclosed in quotes for the JSON query to be a valid JSON. 

Variables are not escaped because we can escape only constants.

The generated SQL statement will include the variables. Variables will be substituted later by constants.
