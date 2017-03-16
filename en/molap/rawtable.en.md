## Create Raw Table

Raw Table is introduced for detailed data query and its creation process is embeded in Cube's. This chapter will introduce Raw Table creation process with example data coming with KAP.

Open KAP Web UI, select project `KAP_Sample_1` in project list located at upper left corner. Cube creation happens on `Cube` page.

![](images/createcube_1.png)

Raw Table definition is in Step 6 and it's disabled by default. 

![](images/createrawtable_1.jpg)

Selecting `Config Raw Table` shows the default Raw Table definition.

![](images/createrawtable_2.jpg)

### Encoding

Click `Encoding` dropdown on each row, user can select encoding for each line. The default encoding is `orderedbytes`.

![](images/createrawtable_3.jpg)


1. `orderedbytes` encoding is designed for all types and the default choice. It keeps data's order when encoding. It's the default encoding type in most cases.
2. `var` encoding is similar to orderedbytes except it does not preserve order. It's not suggested any more, please use orderedbytes where applies.
3. `boolean` Use 1 byte to encode boolean values, valid value include: true, false, TRUE, FALSE, True, False, t, f, T, F, yes, no, YES, NO, Yes, No, y, n, Y, N, 1, 0
4. `integer` Use N bytes to encode integer values, where N equals the length parameter and ranges from 1 to 8. [ -2^(8*N-1), 2^(8*N-1)) is supported for integer encoding with length of N. 
5. `int` Deprecated, use latest integer encoding instead. 
6. `date` Use 3 bytes to encode date dimension values. 
7. `time` Use 4 bytes to encode timestamps, supporting from 1970-01-01 00:00:00 to 2038/01/19 03:14:07. Millisecond is ignored. 
8. `fix_length` Use a fixed-length("length" parameter) byte array to encode integer dimension values, with potential value truncations. 
9. `fixed_length_hex` Use a fixed-length("length" parameter) byte array to encode the hex string dimension values, like 1A2BFF or FF00FF, with potential value truncations. Assign one length parameter for every two hex codes.

Notice `dict` encoding is NOT supported.

### Index

Click `Index` dropdown on each row, user can select index type for each line. The default encoding is `discrete`.

![](images/createrawtable_4.jpg)

1. `discrete` index is the default index, it means only equal index is build for this line.
2. `fuzzy` index is for query with `Like` filter. If the column will in `Like` filter, please set the index as `fuzzy`.
3. `sorted` index means the column is sorted and currently there must be one and only one column set as `sorted` in a table. By default, the partition column in data model is picked as `sorted` column.
