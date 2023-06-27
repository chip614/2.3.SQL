# 2.3.SQL
Задание по углубленному изучению SQL 


1. Определить, сколько книг прочитал каждый читатель в текущем году. Вывести рейтинг читателей по убыванию.
SELECT reader_id, count(reader_id) as count_books 
FROM registration 
WHERE YEAR(date)='2023' 
GROUP BY reader_id 
ORDER BY count_books;

2. Определить, сколько книг у читателей на руках на текущую дату.
SELECT sum(*)
FROM registration
WHERE CURRENT_DATE-date<period

3. Определить читателей, у которых на руках определенная книга.
SELECT first_name, last_name FROM readers WHERE reader_id IN (
SELECT reader_id FROM registration WHERE book_id = (
SELECT book_id FROM books WHERE title="Определенная книга")) 

4. Определите, какие книги на руках читателей.
SELECT b.title
FROM books b JOIN registration r ON b.book_id=r.book_id
WHERE CURRENT_DATE-r.date<period

5. Вывести количество должников на текущую дату. 
SELECT sum(reader_id)
FROM registration
WHERE CURRENT_DATE-date<period
GROUP BY reader_id

6. Книги какого издательства были самыми востребованными у читателей? Отсортируйте издательства по убыванию востребованности книг.
SELECT b.publisher, count(*) as count_pub
FROM books b JOIN registration r ON b.book_id=r.book_id
GROUP BY b.publisher
ORDER BY count_pub DESC

7. Определить самого издаваемого автора.
SELECT authors, count(*) as authors_count
FROM books
GROUP by authors
ORDER BY authors_count DESC
LIMIT 1

8. Определить среднее количество прочитанных страниц читателем за день.
SELECT avg(sum_pages) FROM (
SELECT sum(b.pages) as sum_pages
FROM books b JOIN registration r ON b.book_id=r.book_id
GROUP BY reader_id, date)
