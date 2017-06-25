## 基本SQL支持
KAP支持ANSI SQL 2003*，以下列举了支持的基本SQL语句。

### 支持语法

```sql
statement:
|  query 

query:
      values
  |  WITH withItem [ , withItem ]* query
  |   {
          select
      |  selectWithoutFrom
      |  query UNION [ ALL | DISTINCT ] query
      |  query INTERSECT [ ALL | DISTINCT ] query
      }
      [ ORDER BY orderItem [, orderItem ]* ]
      [ LIMIT { count | ALL } ]
      [ OFFSET start { ROW | ROWS } ]
      [ FETCH { FIRST | NEXT } [ count ] { ROW| ROWS } ]

withItem:
      name
      ['(' column [, column ]* ')' ]
      AS '(' query ')'

orderItem:
      expression [ ASC | DESC ]［ NULLS FIRST |NULLS LAST ］

select:
      SELECT [ ALL | DISTINCT]
          { * | projectItem [, projectItem ]* }
      FROM tableExpression
      [ WHERE booleanExpression ]
      [ GROUP BY { groupItem [, groupItem ]* }]
      [ HAVING booleanExpression ]
      [ WINDOW windowName AS windowSpec [,windowName AS windowSpec ]* ]

selectWithoutFrom:
      SELECT [ ALL | DISTINCT ]
          { * | projectItem [, projectItem ]* }

projectItem:
      expression [ [ AS ] columnAlias ]
  |  tableAlias . *

tableExpression:
      tableReference [, tableReference ]*
  |  tableExpression [ NATURAL ]［( LEFT | RIGHT | FULL ) [ OUTER ] ］ JOINtableExpression [ joinCondition ]

joinCondition:
      ON booleanExpression
  |  USING '(' column [, column ]* ')'

tableReference:
      tablePrimary
      [ matchRecognize ]
      [ [ AS ] alias [ '(' columnAlias [,columnAlias ]* ')' ] ]

tablePrimary:
      [ [ catalogName . ] schemaName . ]tableName
      '(' TABLE [ [ catalogName . ] schemaName. ] tableName ')'
  |   [LATERAL ] '(' query ')'
  |  UNNEST '(' expression ')' [ WITH ORDINALITY ]
  |   [LATERAL ] TABLE '(' [ SPECIFIC ] functionName '(' expression [, expression ]*')' ')'

values:
      VALUES expression [, expression ]*
 
groupItem:
      expression
  |   '('')'
  |   '('expression [, expression ]* ')'
  |  GROUPING SETS '(' groupItem [, groupItem ]* ')'

windowRef:
      windowName
  |  windowSpec

windowSpec:
      [windowName ]
      '(' 
      [ ORDER BYorderItem [, orderItem ]* ]
      [ PARTITION BY expression [, expression]* ]
      [
          RANGE numericOrIntervalExpression {PRECEDING | FOLLOWING }
      |  ROWS numericExpression { PRECEDING | FOLLOWING }
      ]
    ')'
```

*不同版本实现反馈可能会有不同，如有问题请咨询技术服务团队。