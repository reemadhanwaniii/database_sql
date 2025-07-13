partitioning :
<img width="596" height="317" alt="image" src="https://github.com/user-attachments/assets/efa57c74-c64c-4625-acd2-be919afcb228" />
partition pruning is not working here it is scanning all partitions because we use expression (year*100 + month)


use ecommerce;

create table booking(
    booking_id INT,
    booking_status VARCHAR(30),
    booking_date DATE,
    user_id int,
    PRIMARY KEY(booking_id,booking_date)
)

PARTITION BY RANGE(YEAR(booking_date)*100 + MONTH(booking_date)) (
    PARTITION p202501 VALUES LESS THAN (202501),
    PARTITION p202502 VALUES LESS THAN (202502),
    PARTITION p202503 VALUES LESS THAN (202503),
    PARTITION p202504 VALUES LESS THAN (202504),
    PARTITION p202505 VALUES LESS THAN (202505),
    PARTITION p202506 VALUES LESS THAN (202506),
    PARTITION pmax VALUES LESS THAN MAXVALUE
);
   
insert into booking(booking_id,user_id,booking_date,booking_status)
            values (1,101,"2025-01-12","confirmed"),
                   (2,102,"2025-01-03","pending"),
                   (3,103,"2025-02-15","Cancelled"),
                   (4,104,"2025-03-12","Confirmed"),
                   (5,105,"2025-04-05","Pending"),
                   (6,106,"2025-05-17","confirmed"),
                   (7,107,"2025-06-02","cancelled"),
                   (8,108,"2025-07-10","pending"),
                   (9,109,"2025-08-12","confirmed"),
                   (10,110,"2025-03-09","pending");


select * from booking where booking_date BETWEEN '2025-01-01' and '2025-12-31';

SELECT
    TABLE_NAME, PARTITION_NAME, TABLE_ROWS
FROM
    information_schema.PARTITIONS
WHERE
    TABLE_NAME = 'booking';

EXPLAIN SELECT * FROM booking 
WHERE YEAR(booking_date)*100 + MONTH(booking_date) = "202503";

EXPLAIN SELECT * FROM booking
WHERE booking_date BETWEEN '2025-03-01' AND '2025-03-31';

ALTER TABLE booking
ADD PARTITION (
    PARTITION p202509 VALUES LESS THAN (202510)
);
## 
In MySQL, the partition key must be part of every UNIQUE/PRIMARY key.

n MySQL partitioning, conditions like LESS THAN are a bit tricky — because MySQL doesn't support LESS THAN OR EQUAL TO directly in partition definitions<img width="1048" height="604" alt="image" src="https://github.com/user-attachments/assets/0e56ca84-cff5-4e45-952b-dcd95adfd5ef" />





<img width="464" height="77" alt="image" src="https://github.com/user-attachments/assets/c781c59b-ffd3-4032-b21f-ed55d58e0e05" />


In MySQL:

MAXVALUE means: "this is the last possible partition"

So it must always be the final one

You cannot add a partition after it, because nothing is supposed to go beyond MAXVALUE

 Solution: Drop pmax, then re-add it

## Creating views 

CREATE VIEW booking_2025 AS
SELECT
    TABLE_NAME, PARTITION_NAME, TABLE_ROWS
FROM
    information_schema.PARTITIONS
WHERE
    TABLE_NAME = 'booking';


select * from booking_2025;
