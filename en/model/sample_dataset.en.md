## Sample Data Set

The KAP binary package contains a sample data set for testing, total size is about 1.5 MB. A total of five tables, including the fact table which has 10 000 rows. Because of the small data size, it is convenient to carry out as a test in virtual machine.

KAP supports both star schema and snowflake data model since v2.4.0. At this article, we will use a typical star schema as our sample data set which contains five tables:

* KYLIN\_SALES
  This is the fact table, it contains detail information of sales orders. Each row holds information such as the seller, the commodity classification, the amount of orders, the quantity of goods and so on. Each row corresponds to a transaction.
* KYLIN\_CATEGORY\_GROUPINGS
  This is a dimension table, it represents details of commodity classification. For example, name of commodity category, etc..
* KYLIN\_CAL\_DT
  This is another dimension table, it presents extended information of dates. Such as the beginning of the year, the beginning of the month, the beginning of week, that a single date falls in.
* DEFAULT.KYLIN_ACCOUNT
  This is user account table. Each row represents a user who could be a buyer and/or a seller of a transaction. Links to KYLIN\_SALES through the BUYER_ID or SELLER_ID.
* DEFAULT.KYLIN_COUNTRY
  The country dimension table. Links to KYLIN_ACCOUNT.

The five tables together constitute the structure of the entire star model. Below is a relational diagram of them.

![](images/dataset_1.png)

## Data Dictionary

Below introduces some key columns in the tables.

| Table                      | Field                 | Description                      |
| :------------------------- | :-------------------- | :------------------------------- |
| KYLIN\_SALES               | PART\_DT              | Order Date                       |
| KYLIN\_SALES               | LEAF\_CATEG\_ID       | ID Of Commodity Category         |
| KYLIN\_SALES               | SELLER\_ID            | Account ID Of Seller             |
| KYLIN\_SALES               | BUYER\_ID             | Account ID Of Buyer              |
| KYLIN\_SALES               | PRICE                 | Order Amount                     |
| KYLIN\_SALES               | ITEM\_COUNT           | The Number Of Purchased Goods    |
| KYLIN\_SALES               | LSTG\_FORMAT\_NAME    | Order Transaction Type           |
| KYLIN\_CATEGORY\_GROUPINGS | USER\_DEFINED\_FIELD1 | User Defined Fields 1            |
| KYLIN\_CATEGORY\_GROUPINGS | USER\_DEFINED\_FIELD3 | User Defined Fields 3            |
| KYLIN\_CATEGORY\_GROUPINGS | UPD\_DATE             | Update Date                      |
| KYLIN\_CATEGORY\_GROUPINGS | UPD\_USER             | Update User                      |
| KYLIN\_CATEGORY\_GROUPINGS | META\_CATEG\_NAME     | Level1 Category                  |
| KYLIN\_CATEGORY\_GROUPINGS | CATEG\_LVL2\_NAME     | Level2 Category                  |
| KYLIN\_CATEGORY\_GROUPINGS | CATEG\_LVL3\_NAME     | Level3 Category                  |
| KYLIN\_CAL\_DT             | CAL\_DT               | Date                             |
| KYLIN\_CAL\_DT             | WEEK\_BEG\_DT         | Week Beginning Date              |
| KYLIN\_CAL\_DT             | MONTH\_BEG\_DT        | Month Beginning Date             |
| KYLIN\_CAL\_DT             | YEAR\_BEG\_DT         | Year Beginning Date              |
| KYLIN\_ACCOUNT             | ACCOUNT\_ID           | ID Number Of Account             |
| KYLIN\_ACCOUNT             | ACCOUNT\_COUNTRY      | Country ID Where Account Resides |
| KYLIN\_COUNTRY             | COUNTRY               | Country ID   (2 chars)           |
| KYLIN\_COUNTRY             | NAME                  | Descriptive Name Of Country      |
