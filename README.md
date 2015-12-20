Sampling for MySQL
======================

An extension of mysql 5.7 for uniform sampling.



Syntax
------

    select column1, avg(column2)
    from table1
    where column1 > value
    group by column1
    under sampling rate <rate>;


If <rate> is between [0.0, 1.0], it refers to the percentage of the result set to be sampled. If <rate> is greater than 1, it does not sample, just return entire records. 

Sampling clause can only be in the outer most SELECT clause, and cannot appear in a subquery.

If AVG aggregation function is used in SELECT clause with sampling clause, standard deviation function, aliased with "stddev", is automatically appended at the last position as the screening task description requires. Automatic standard deviation function is supported only with average function.

About the method for performing sampling, sequential random sampling is performed during query execution based on the algorithm S described in the paper ["Faster Methods for Random Sampling"](http://www.mathcs.emory.edu/~cheung/papers/StreamDB/RandomSampling/1984-Vitter-Faster-random-sampling.pdf) by Jeffrey Scott Vitter, and its running time is O(N) where N is the number of total records. This latency can be enhanced by choosing another best algorithm from the paper depending on the number of total records and requested sample counts.


Demo
----

    host: 52.192.18.126
    port: 3306
    username: sampling
    password: sampling
    
Sample query

    USE test_sampling;

    SELECT AVG(salary), department
    FROM employee
    WHERE salary > 5000 
    GROUP BY department
    UNDER SAMPLING RATE 0.3; 
