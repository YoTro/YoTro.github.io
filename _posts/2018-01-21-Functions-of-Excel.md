---
title: A serial of functions in Excel
date: 2018-01-21 12:52:31
layout: post
tags: Excel
---
## Vlookup,IF,ROW,MATCH INDEX SMALL

### IF

> IF(logical_test,value_if_true,value_if_false)

Judge whether a certain conditions, if satisfied, return a value, if not satisfied, return another value


### Vlookup

>Vlookup(value,table_array,col_index_num,[range_lookup])

Search the table area to the first element meet the conditions , determines the line number of cells in the area to be retrieved, and then returns a chosen cell value. By default, the table is sorted in ascending order.

### ROW

>ROW(refference)

Return a reference line number.

### MATCH/SEARCH

>MATCH(lookup_value,lookup_array,[math_type])

Returns the relative position of items that meet a particular order and specific values in an array

### INDEX

>INDEX(array,row_num,[column_num])
>INDEX(refference,row_num,[column_num],[area_num])

In a given range of cells, returns the cell's value or reference at the intersection of a particular row and column

### SMALL

>SMALL(array,k)

Returns the kth minimum value in the array
