create DATABASE indexingdemo;
use indexingdemo;

create table users(
    id INT PRIMARY KEY,
    name VARCHAR(100),
    email VARCHAR(100),
    age INT,
    city VARCHAR(50)
)


select * from users where city="Bhopal";


CREATE INDEX idx_city ON users(city);

EXPLAIN SELECT * FROM users WHERE city = 'Bhopal';
SHOW INDEX from users;
<img width="710" height="179" alt="image" src="https://github.com/user-attachments/assets/60498f3e-8130-4472-86ef-3f58c305b87d" />


SHOW CREATE TABLE users;
<img width="415" height="145" alt="image" src="https://github.com/user-attachments/assets/b485c614-ecb2-4052-9407-3d4876a28fb6" />
